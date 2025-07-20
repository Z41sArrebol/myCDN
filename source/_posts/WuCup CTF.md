---
title: 2024 WuCup CTF wp
date: 2024-12-02 19:00:26
tags: 
    - CTF
    - wp
categories: CTF
cover: /img/landscape/风景5.jpg
---

0xGame作为笔者的启蒙赛事，笔者从中学习到了一些CTF的基本知识，而WuCup则是笔者第一个独立参加的网络赛事。

虽然只做出来四道题，但是99%的步骤都是本人完成))

恰逢笔者的博客基本搭建完成，所以兴奋的把本来用PDF提交的wp转成.md格式扔到这里

# Web-Sign

开启题目环境后，使用webshell工具中国蚁剑连接

题目环境已经告知密码passwd为sgin

![](/img/WuCup/WuCup1.png)

连接后打开最上层的目录，在最底下发现flag的文件夹

![](/img/WuCup/WuCup2.png)
![](/img/WuCup/WuCup3.png)


WuCup{253e7c29-a436-4f17-bce2-72ce13731ba3}

# Crypto-easy

观察应该是RC4加密，直接扔kimi写脚本

```python
key = "hello world"

# 初始化s和t数组
s = list(range(256))
j = 0
t = [ord(c) for c in key] * (256 // len(key)) + [ord(c) for c in key][:(256 % len(key))]

# RC4算法生成密钥流
for i in range(256):
    j = (j + s[i] + t[i]) % 256
    s[i], s[j] = s[j], s[i]

# 用于解密的索引
i = j = 0
flag_encrypted = [
    0xd8, 0xd2, 0x96, 0x3e, 0x0d, 0x8a, 0xb8, 0x53, 0x3d, 0x2a, 0x7f, 0xe2, 0x96, 0xc5, 0x29, 0x23,
    0x39, 0x24, 0x6e, 0xba, 0x0d, 0x29, 0x2d, 0x57, 0x52, 0x57, 0x83, 0x59, 0x32, 0x2c, 0x3a, 0x77,
    0x89, 0x2d, 0xfa, 0x72, 0x61, 0xb8, 0x4f
]

# 解密flag
flag_decrypted = []
for byte in flag_encrypted:
    i = (i + 1) % 256
    j = (j + s[i]) % 256
    s[i], s[j] = s[j], s[i]
    x = (s[i] + s[j]) % 256
    flag_decrypted.append(byte ^ s[x])

# 将解密后的bytes转换为字符串
flag = ''.join(chr(b) for b in flag_decrypted)
print(flag)
```

最后扔到编译器运行就行

![](/img/WuCup/WuCup4.png)

WuCup{55a0a84f86a6ad40006f014619577ad3}

# Misc-原神启动

打开压缩包之后只有一张图片和一个加密的word文档

Winhex打开发现就是正常的png文件，考虑其他隐写方式

扔到stegsolve里面看看

![](/img/WuCup/WuCup5.png)

很清楚的打开了第一层加密，里面是一个word文档，打开后只有 原神启动！这几个字

网上搜索，改成.zip格式再进行分析

分析压缩包，发现在word/media目录下有一个image1.png图片，打开很容易发现一行字

![](/img/WuCup/WuCup6.png)

为了更清楚一点，还是扔到stegsolve进行处理

![](/img/WuCup/WuCup7.png)

此为第二层加密

打开后，发现还有一个被加密的txt文件，因为没有发现其他线索，优先考虑伪加密

扔到winhex里面，发现只是正常的加密

故重新返回目录.xml文件进行寻找

考虑到一二层的密码都为WuCup{ }格式，猜想第三层密码也相同，但在所有文件搜索WuCup都没有结果

于是缩小范围，搜索Wu和"{"，最终在document.xml中发现了线索（标蓝色的）

![](/img/WuCup/WuCup8.png)

仔细往后看不难发现所有的元素组合起来即为一个标准的flag格式（如下图）

![](/img/WuCup/WuCup9.png)

（此处换了个编辑器，显示的会更明显）

组合起来可得到第三层密码：WuCup{f848566c-3fb6-4bfd-805a-d9e102511784}

输入密码得到txt文件

![](/img/WuCup/WuCup10.png)

WuCup{0e49b776-b732-4242-b91c-8c513a1f12ce}

# Misc-Sign

签到题，十六进制的字符串，扔cyberchef里面直接出

![](/img/WuCup/WuCup11.png)

WuCup{df357d47-31cb-42a8-aa0c-6430634ddf4a}