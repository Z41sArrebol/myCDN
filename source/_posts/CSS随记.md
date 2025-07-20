---
title: CSS随记
author: Z41sArrebol
date: 2025-02-18 23:19:02
tags: 
    - 前端
    - CSS
cover: /img/girls/星河少女.jpeg
categories: 前端开发
---
## 元素

1.一个元素由开始标签，内容以及结束标签组成。

2.如果是单独呈现的标签，一个标签就代表一个元素

3.标签名对大小写不敏感

## 运行和预览前端工程

右上角的terminal -> new terminal -> serve . --listen 8080 -> 在预览中开启8080端口

## html文档的基本结构

​![image](img/CSS1/111.png)​

## 标签

<p></p> 段落标签

<h1></h1>一级标题 同理可编辑1-6级

<strong></strong> 对内容进行加粗

​![image](img/CSS1/image-20250119130010-pplwgo0.png)​

## CSS引入

```html
<link rel="stylesheet" href="./index.css">
<!-- 或者./去掉也可以，效果是一样的 -->
<link rel="stylesheet" href="index.css">
```

‍

span和div互相转化：display：inline/block

display：none 即隐藏

## 常见写法：父子类

```css
<div class="box">
        <img src="https://gw.alicdn.com/bao/uploaded/i1/749716436/O1CN01OSPWqa1xPjeErSSzf_!!749716436-0-pixelsss.jpg">
        <div class="introduce">男运动鞋</div>
        <div class="price">¥78.3</div>
</div>
```

```css
<div class="father">
  <div class="box1"></div>
  <div class="box2"></div>
</div>
```

## div元素行内置

​![image](img/CSS1/image-20250218103506-n2kwhki.png)​

解决方法：

1.div元素写在一行

2.font-size:0px

3.word-spacing<-20px即可

## 垂直居中

```css
  height: 30px;
  line-height: 30px;
```

## 边框倒角

​`border-radius: 10px;`​

圆形图片：`border-radius: 50%;`​

## 右下移动图片

使用padding：会改变下一张图片的位置

正确方法：使用position:relative （相对定位）  top left... 遵循文档流的排列

## absolute

绝对定位 不为元素预留空间 变成第二个图层再做出相应改变

​![image](img/CSS1/image-20250218185201-4gbqja3.png)​

## relative调位

​![image](img/CSS1/image-20250218190303-0pce4sk.png)​

注意的是把圆形放到右上角，需要做的是

```css
.select {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  box-sizing: border-box;
  border: 2px solid #bfbfbf;
  position: relative;
  bottom: 240px;
  left: 150px;
}
```

## 图层优先级

​![image](img/CSS1/image-20250218190911-8ki56wa.png)​

## float

适用对象：一左一右/文字环绕图片

## 渐变颜色

linear-gradient

例：background:linear-gradient(to right, #95ca47 30%, #4dc891 70%)

​![image](img/CSS1/image-20250218210349-u79f62a.png)​

## 背景图片

background-image: url(图片地址)

注意：图片大小如果没有充满容器，图片会一直重复

需要用到background-repeat：no-repeat来禁止

    另外有：repeat（默认值）repeat-x/y（只在水平/垂直方向重复）

背景图居中，用到background-position: center;

​![image](img/CSS1/image-20250218211113-t8qcwjj.png)​

background-size

​![image](img/CSS1/image-20250218211257-ax7ym6e.png)​

图片背景属性简写：
![image](img/CSS1/image-20250218211521-96a8ag5.png)​

## 模态框

1.一直处于浏览界面中心

2.有一个半透明的背景

​![image](img/CSS1/image-20250218211913-bjd6u9a.png)​

​![image](img/CSS1/image-20250218212040-18s9qzk.png)​
