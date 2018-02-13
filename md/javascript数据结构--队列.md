---
title: JS数据结构--队列
date: 2018-01-03 16:19:47
tags: 
	- 数据结构
	- JS
---

队列（Queue） ,和栈非常类似，但使用不用原则，即 先进先出 。
例子：排队
![排队（队列），先进先出](http://upload-images.jianshu.io/upload_images/5287253-a3c1a3775a6bab63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

创建队列
（和创建栈很类似，不详细讲，可参考[javascript数据结构--栈](https://www.jianshu.com/p/c5112f753277)）



```
function Queue() {

    var items = [];
    //  队列尾部添加新项
    this.enqueue = function(element){
        items.push(element);
    };
     //移除并返回队列第一项
    this.dequeue = function(){
        return items.shift();
    };

    this.front = function(){
        return items[0];
    };

    this.isEmpty = function(){
        return items.length == 0;
    };

    this.clear = function(){
        items = [];
    };

    this.size = function(){
        return items.length;
    };

    this.print = function(){
        console.log(items.toString());
    };
}

```

Queue类和Stack类非常类似，唯一区别是dequeue 方法和front方法的不用，由先进先出，后进先出原则不同造成。

* 使用

```
var queue = new Queue(); console.log(queue.isEmpty()); //؜true
queue.enqueue("John"); 
queue.enqueue("Jack");
queue.enqueue("Camila");
queue.print();//John,Jack,Camila
queue.dequeue(); 
queue.dequeue(); 
queue.print();//Camila
```

![添加](http://upload-images.jianshu.io/upload_images/5287253-23bcbef37f4fff23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![删除](http://upload-images.jianshu.io/upload_images/5287253-436b69a7412d023b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
