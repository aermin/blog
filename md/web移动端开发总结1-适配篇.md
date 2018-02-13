---
title: web移动端开发总结1--适配篇
type: tags
date: 2017-10-10 15:05:47
tags:
		- 开发总结
		- web移动端
---

在公司主要写web移动端的项目，一开始较大的感触就是适配很麻烦，分ios和安卓，安卓生态又混乱得很，所以适配要做好了，不然这个设备好好的，有些设备却页面错乱。

在网上找了很多方案，踩了不少坑。

方案一：

```
(function (doc, win) {
          console.log("dpr:"+win.devicePixelRatio); 
          var docEle = doc.documentElement,
              isIos = navigator.userAgent.match(/iphone|ipod|ipad/gi),
              dpr=Math.min(win.devicePixelRatio, 3);
              scale = 1 / dpr,

              resizeEvent = 'orientationchange' in window ? 'orientationchange' : 'resize';

          docEle.dataset.dpr = dpr;

          var metaEle = doc.createElement('meta');
          metaEle.name = 'viewport';
          metaEle.content = 'initial-scale=' + scale + ',maximum-scale=' + scale;
          docEle.firstElementChild.appendChild(metaEle);
          

          var recalCulate = function () {
                  var width = docEle.clientWidth;
                  if (width / dpr > 640) {
                      width = 640 * dpr;
                   }
                docEle.style.fontSize = 20 * (width / 750) + 'px';
            };

          recalCulate()

          if (!doc.addEventListener) return;
          win.addEventListener(resizeEvent, recalCulate, false);
        })(document, window);
```

 <!-- more -->

获取设备dpr
算出缩放比例 scale = 1/dpr
创建meta以及属性
将scale值赋给initial-scale，maximum-scale
meta插入到文档中
创建屏幕大小改变重新计算函数并监听

特点：这个方案根据设备等比例缩放，每个设备显示内容一致。
 缺点：当我用这套方案时，有个问题，因为监听resizeEvent，导致页面打开会先内容变大，然后再正常显示，很是影响用户体验。
[参考链接](http://www.cnblogs.com/leinov/p/5209456.html)


方案二（推荐）：

```
    //获取屏幕比例
    function sreenRatio() {
        const ua = navigator.userAgent;
        const matches = ua.match(/Android[\S\s]+AppleWebkit\/(\d{3})/i);
        const UCversion = ua.match(/U3\/((\d+|\.){5,})/i);
        const isUCHd = UCversion && parseInt(UCversion[1].split('.').join(''), 10) >= 80;
        const isIos = navigator.appVersion.match(/(iphone|ipad|ipod)/gi);
        var dpr = window.devicePixelRatio || 1;
        if (!isIos && !(matches && matches[1] > 534) && !isUCHd) {
            // 如果非iOS, 非Android4.3以上, 非UC内核, 就不执行高清, dpr设为1;
            dpr = 1;
        }
        return dpr;
    }
    //初始化屏幕比例
    function screenRatio(baseFontSize, fontscale) {
        var ratio = sreenRatio();     
        var scale = document.createElement('meta');
        var scaleRatio = 1 / ratio;
        scale.name = 'viewport';
        scale.content = 'width=device-width,'+'initial-scale=' + scaleRatio + ', maximum-scale=' + scaleRatio + ', minimum-scale=' +
            scaleRatio + ', user-scalable=no';
        var s = document.getElementsByTagName('title')[0];
        s.parentNode.insertBefore(scale, s);
        var _baseFontSize = baseFontSize || 100;
        var _fontscale = fontscale || 1;
        document.documentElement.style.fontSize = _baseFontSize / 2 * ratio * _fontscale+'px';
    }
        if (window.screen.width >= 768) {
            screenRatio(100, 1.5);//字体放大1.5倍
        } else {
            screenRatio();
        }
```

特点：

*  引用简单，布局简便
* 根据设备屏幕的DPR,自动设置最合适的高清缩放。
* 保证了不同设备下视觉体验的一致性。（老方案是，屏幕越大元素越大；此方案是，屏幕越大，看的越多）
* 有效解决移动端真实1px问题（这里的1px 是设备屏幕上的物理像素）
ps：而且不会出现方案一的问题

缺点：1.有可能会出现字体会不受控制的变大的情况，解决方法：css加上一下内容

```
*, *:before, *:after { max-height: 100000px }
```

2. 感觉没啥问题了，然而我司测试硬生生发现一个bug -> 在某安卓设备发现在QQ上打开网页出现页面错乱。解决方法：判断如果是安卓设备，scale.content加上target-densitydpi=device-dpi

修正：

```
    //获取屏幕比例
    function sreenRatio() {
        const ua = navigator.userAgent;
        const matches = ua.match(/Android[\S\s]+AppleWebkit\/(\d{3})/i);
        const UCversion = ua.match(/U3\/((\d+|\.){5,})/i);
        const isUCHd = UCversion && parseInt(UCversion[1].split('.').join(''), 10) >= 80;
        const isIos = navigator.appVersion.match(/(iphone|ipad|ipod)/gi);
        var dpr = window.devicePixelRatio || 1;
        if (!isIos && !(matches && matches[1] > 534) && !isUCHd) {
            // 如果非iOS, 非Android4.3以上, 非UC内核, 就不执行高清, dpr设为1;
            dpr = 1;
        }
        return dpr;
    }
    //初始化屏幕比例
    function screenRatio(baseFontSize, fontscale) {
        var ratio = sreenRatio();
        var scale = document.createElement('meta');
        var scaleRatio = 1 / ratio;
        scale.name = 'viewport';
		<%/*安卓设备兼容*/%>
		if (/Android/i.test(navigator.userAgent) == true) {
			scale.content = 'width=device-width, target-densitydpi=device-dpi,'+' initial-scale=' + scaleRatio + ', maximum-scale=' + scaleRatio + ', minimum-scale=' +
	            scaleRatio + ', user-scalable=no';
		<%/*iOS设备*/%>
		} else {
			scale.content = 'width=device-width,'+'initial-scale=' + scaleRatio + ', maximum-scale=' + scaleRatio + ', minimum-scale=' +
	            scaleRatio + ', user-scalable=no';
		}
        var s = document.getElementsByTagName('title')[0];
        s.parentNode.insertBefore(scale, s);
        var _baseFontSize = baseFontSize || 100;
        var _fontscale = fontscale || 1;
        document.documentElement.style.fontSize = _baseFontSize / 2 * ratio * _fontscale+'px';
    }
	var isAndroid = /Android/i.test(navigator.userAgent) ? true : false;
	<%/*安卓设备不做高清放大处理*/%>
    if (window.screen.width >= 768 && !isAndroid) {
        screenRatio(null, 1.5);<%/*字体放大1.5倍*/%>
    } else {
        screenRatio();
    }
```

[参考链接](http://www.jianshu.com/p/985d26b40199)