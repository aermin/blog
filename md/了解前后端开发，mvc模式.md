---
title: 前后端开发，mvc模式
type: tags
date: 2017-8-28 10:21:32
tags: 
     - JS
     - PHP
---

实习两个月除了写前端，还写了点后端(nodejs，php)，前阶段自己也写了个前后端的项目，不过没开源。现在记录下自己对全端开发的个人心得(菜鸟入门级别的，错了请大佬们指导更正)。

感觉不管是php还是nodejs，都是差不多的，大概的作用是处理前端的数据请求，处理数据，操作数据库，把数据返回给前端。

用mvc模式去写一个全端站点。

<!-- more -->

mvc：模型（Model）、视图（View）和控制器（Controller）。
![mvc](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/MVC-Process.svg/500px-MVC-Process.svg.png)

![nodejs(koa)](http://upload-images.jianshu.io/upload_images/5287253-718dfa04e0488f65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

render(view,model)

model可以单独抽出来放一个文件夹


![php(yii)](http://upload-images.jianshu.io/upload_images/5287253-c7f05140b083e4ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个就是吧model抽出来，调用model方法，返回数据，render到页面
。
和nodejs一样的。