---
title: '图文解析HTMLElement.offsetHeight,scrollHeight,clientHeight'
type: tags
date: 2017-06-05 22:32:29
tags: 
      - 学习总结
      - HTML
---


### HTMLElement.offsetHeight
    
是一个只读属性，它返回该元素的像素高度，高度包含该元素的垂直内边距和边框，且是一个整数。

通常，元素的offsetHeight是一种衡量标准，包括元素的边框、垂直内边距和元素的水平滚动条（如果存在且渲染的话）和元素的CSS高度。

对于文档的主体对象，它包括代替元素的CSS高度线性总含量高。浮动元素的向下延伸内容高度是被忽略的。 

![实例](https://developer.mozilla.org/@api/deki/files/788/=OffsetHeight.png)

<!-- more -->

### Element.scrollHeight

Element.scrollHeight 是计量元素内容高度的只读属性，包括overflow样式属性导致的视图中不可见内容。没有垂直滚动条的情况下，scrollHeight值与元素视图填充所有内容所需要的最小值clientHeight相同。包括元素的padding，但不包括元素的margin.

![实例](https://developer.mozilla.org/@api/deki/files/840/=ScrollHeight.png)

### Element.clientHeight

返回元素内部的高度(单位像素)，包含内边距，但不包括水平滚动条、边框和外边距。

clientHeight 可以通过 CSS height + CSS padding - 水平滚动条高度 (如果存在)来计算.

也就是说，是没有垂直滚动条版本的scrollHeight。

---------------------------------
更新：

clientWidth = width + padding

clientHeight = height + padding

offsetWidth = width + padding + border

offsetHeight = height + padding + border

