---
title: 解决非chrome浏览器（移动端适用）后退刷新问题
type: tags
date: 2017-08-08 10:10:32
tags: 
     - 功能
     - 开发总结
---

写页面时你可能会遇到这个问题，就是用Firefox，Safari，IE等非chrome浏览器，点击浏览器自带的返回键会发现不会刷新页面，因为那时js代码没有执行。
> 在网上搜到的有的不能用，有的兼容性很差，故自己想了解决方法，核心是利用setInterval的特性。

这样比如下面这种情形：
本来只建了一个二维码，新建了一个

<!-- more -->

![E4294505-FFE4-473E-9316-5E32A64DB063.png](http://upload-images.jianshu.io/upload_images/5287253-63828908bed31df6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


新建完用户没有点击你写的返回按钮（图中的完成按钮），而是点击了浏览器自带的返回键
> 图片中的chrome浏览器只是当演示用，现实中请用非chrome的去感受这个问题
比如[用firefox点击打开图片示例的网站链接](http://www.topscan.com/dongtaima/dcode)

![736A09E5-D8C0-45E9-90CC-27F4AE5378B5.png](http://upload-images.jianshu.io/upload_images/5287253-0e27f0d8813c94a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

结果回去页面没有刷新，结果显示还是原来的样子
![736A09E5-D8C0-45E9-90CC-27F4AE5378B5.png](http://upload-images.jianshu.io/upload_images/5287253-a04bfa95d5a8602f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而如果返回刷新了，会这样（用户体验会不会好些？）
![9F4837FE-BF66-437F-B2D7-67FE88229866.png](http://upload-images.jianshu.io/upload_images/5287253-8c6658785c2d8222.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


奉上代码
> 要刷新的页面（如页面A）
```
<script>
    //chrome自带后退刷新，故不再次刷新
       var ua = window.navigator.userAgent; 
       var isChrome = ua.indexOf("Chrome") && window.chrome;  
       if (! isChrome) {  
    //浏览器后退刷新
            function reload() {
                setInterval(function() {   //这个定时器返回A页面会继续执行
                    if (localStorage.reload == 'true' ) {  //判断是否刷新页面
                        localStorage.setItem('reload','false');
                        location.reload()
                    }
                }, 500)
            };
        reload();
       }  

</script>
```
>在A页面之后访问的页面（如页面B）添加一下一行代码
ps：作为A页面执行刷新功能的开关
```
localStorage.setItem('reload','true');
```