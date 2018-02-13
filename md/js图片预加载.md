---
title: js图片预加载
type: tags
date: 2017-07-10 20:21:32
tags: 
     - JS
     - 学习总结
---

继上一次的[图片懒加载](http://www.jianshu.com/p/52a85f91d070)，这次讲下js实现图片预加载。

> 先上demo

[demo:图片预加载](http://www.hxvin.me/PracticeJS/preload/preload.html)

>再上代码

<!-- more -->

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        (function () {
            function loadImages(sources, callback) {
                var count = 0,
                    images = {},
                    imgNum = 0;
                for (src in sources) {
                    imgNum++;   //imgNum =2
                }
                for (src in sources) { //src 为 img1和img2
                    images[src] = new Image();  //images = {img1:img,img2:img}
                    images[src].onload = function () { //onload 事件会在页面或图像加载完成后立即发生
                        if (++count >= imgNum) { //当次数大于等于2次时
                            callback(images);  //调用loadImages的回调函数
                        }
                    }
                    images[src].src = sources[src]; //img1-> img.src = "http://pic.58pic.com/58pic/17/18/97/01U58PIC4Xr_1024.jpg";img2同理
                }
            }
            //存储图片链接信息的关联数组  
            var sources = { //想要多少张就放多少张
                img1: "http://pic.58pic.com/58pic/17/18/97/01U58PIC4Xr_1024.jpg",
                img2: "http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg",
                img3:"http://ww1.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg"
            };
            //调用图片预加载函数，实现回调函数  
            loadImages(sources, function (images) {
                //TO-DO something  (如下面用canvas把图画出来)
                //var images = new Image();   创建一个<img>元素
                // images.src = 'myImage.png';  设置图片源地址
                var canvas = document.getElementById('canvas');
                var ctx = canvas.getContext('2d');
                //ctx.drawImage(image, dx, dy, dWidth, dHeight);
                ctx.drawImage(images.img1, 20, 20, 100, 100);
                ctx.drawImage(images.img2, 140, 20, 100, 100); 
                ctx.drawImage(images.img3, 260, 20, 100, 100); 
            });
        }());
    </script>
</head>
<body>
    <canvas id="canvas" width="1000px" height="1000px"></canvas>
</body>
</html>
```

可以看出代码注释很详细，
如果有些知识点还不是很清楚，请看我找的一些好的知识讲解文章,省得像我一样到处找啦；

同时，如果您觉得有点帮助，可以关注给我的github项目给个start，watch哦，我会不定期更新js的一些开发常用的知识点技能。

[js开发常见技能收集->github](https://github.com/hxvin/PracticeJS)


[onload](http://www.w3school.com.cn/jsref/event_onload.asp)

[for in 遍历](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)

[Image 元素构造器](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement/Image)

[为何使用++count](https://www.w3cplus.com/javascript/javascript-increment-and-decrement-operatorssass.html)

[如何使用canvas创建图片](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Using_images)

[canvas的drawImage()](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/drawImage)

如有错误，恳请指出指导^ ^