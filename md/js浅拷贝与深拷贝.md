---
title: js浅拷贝与深拷贝
type: tags
date: 2017-06-18 22:22:12
tags: 
     - JS
     - 学习总结
---

深复制和浅复制只针对像 Object, Array 这样的复杂对象的。简单来说，浅复制只复制一层对象的属性，而深复制则递归复制了所有层级。

> 浅拷贝

```
var obj = { a:1, arr: [2,3] };
var shallowObj = shallowCopy(obj);

function shallowCopy(src) {
  var dst = {};
  for (var prop in src) {
    if (src.hasOwnProperty(prop)) {
      dst[prop] = src[prop];
    }
  }
  return dst;
}

```

<!-- more -->

因为浅复制只会将对象的各个属性进行依次复制，并不会进行递归复制，而 JavaScript 存储对象都是存地址的，所以浅复制会导致 obj.arr 和 shallowObj.arr 指向同一块内存地址。

导致的结果就是：
```
shallowObj.arr[1] = 5;
obj.arr[1]   // = 5
```
深复制则不同，它不仅将原对象的各个属性逐个复制出去，而且将原对象各个属性所包含的对象也依次采用深复制的方法递归复制到新对象上。这就不会存在上面 obj 和 shallowObj 的 arr 属性指向同一个对象的问题。

>深拷贝（这个函数可以深拷贝对象和数组）

```
var deepCopy = function(obj){
    var str, newobj = obj.constructor === Array ? [] : {};
    if(typeof obj !== 'object'){
        return;
    } else if(window.JSON){
        str = JSON.stringify(obj), //系列化对象
        newobj = JSON.parse(str); //还原
    } else {
        for(var i in obj){
            newobj[i] = typeof obj[i] === 'object' ? 
            deepCopy(obj[i]) : obj[i]; 
        }
    }
    return newobj;
};

```
结果是：

```
var obj = { a:1, arr: [1,2] };
var deepObj = deepCopy(obj);
deepObj.arr[1]=5;
obj.arr[1]; //2

```

[本文参考地址](https://www.zhihu.com/question/23031215)。

