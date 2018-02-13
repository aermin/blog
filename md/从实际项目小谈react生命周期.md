---
title: 从实际项目小谈react生命周期
type: tags
date: 2017-07-15 10:21:32
tags: 
     - React
     - 开发总结
---
#### 前言

 今天写的一个react版的滚动字幕，思路是用js操作展示内容的scrollWidth,然后用setInterval 循环调用function ，function内容大体思路是对文字内容userDOM.style.transform = 'translateX(-'+ i +'px)',每次横向左移动ipx的距离，形成滚动效果，当i>=scrollWidth的时候，将i归零，形成循环滚动。(这里操作了dom，不是很好)


这里先解释一下浏览器scrollWidth，scrollWidth，offsetWidth三种宽度的区别
情况1：
元素内无内容或者内容不超过可视区，滚动不出现或不可用的情况下。
scrollWidth=scrollWidth，两者皆为内容可视区的宽度。
offsetWidth为元素的实际宽度。
情况2：
元素的内容超过可视区，滚动条出现和可用的情况下。
scrollWidth>clientWidth。
scrollWidth为实际内容的宽度。
clientWidth是内容可视区的宽度。
offsetWidth是元素的实际宽度。

<!-- more -->

#### 出问题了  

为了以便调用后端接口前先模拟展示，将静态生成的内容写死的文字内容换成动态加载，还采取了随机生成用户ip和获奖内容，结果获取到的scrollWidth（元素内容的实际宽度，包括被隐藏的部分）只是内容可视区的宽度，显然是不行的，所以无法滚动全部，我们需要的是元素内容的实际宽度。为什么会出现这种情况呢？如果文字内容是静态生成的，内容写死的，并不会出现这种问题。

#### 问题出在哪里？
出在动态加载还要考虑到react的生命周期，componentWillMount(将要挂载)->render(dom渲染)->componentDidMount(已经挂载)，这个滚动函数写在了componentDidMount中，也就是页面dom结构渲染完了文字内容才开始动态生成，结果scrollWidth抓取到的文字内容是空的，只能获取了内容可视区的宽度。
![网上找的一张react生命周期的好图](http://upload-images.jianshu.io/upload_images/5287253-1e0d1ec12f576c19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 问题找出来了，该如何解决呢？

最简单粗暴的方式就是把动态加载文字内容的函数放在componentWillMount(将要挂载)中，在render页面之前就把该部分内容加载完毕，这样render页面的时候scrollWidth将抓取到早已加载好的文字内容了，可是这样会造成打开页面后，页面空白一会，在渲染出页面的内容来，这段空白期间就是componentWillMount加载你的数据内容去了。如果数据内容少还好说，万一量很大了，那就尴尬了，严重影响用户体验。

####更好的解决办法是 
仍把动态加载文字内容的函数放在componentDidMount中，并在此采用window.onload 调用滚动函数，也就是等页面都渲染完了，文字内容也ok了，才会调用滚动函数（scrollWidth，setInterval，transform ）。

这下，解决了生命周期以及用户体验，加载性能的问题。😀

如有错误和不足，恳请指出指导^ ^