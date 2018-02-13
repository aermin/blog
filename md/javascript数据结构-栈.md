---
title: JS数据结构--栈
date: 2018-01-03 16:19:47
tags: 
		- 数据结构
		- JS
---


数组是计算机科学中最常用的数据结构，栈和队列类似数组，但 添加和删除元素时更为可控。

栈(stack)是一种后进先出原则的有序集合。（如一摞书，后放进去的先拿出来）
![栈](http://upload-images.jianshu.io/upload_images/5287253-59e833ea0bba5e70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

```
function Stack() {
    //用一种数据结构保存栈里的元素，这里选择数组
    var items = []; 
    //添加一个一个或几个元素到栈顶
    this.push = function(element){
        items.push(element);
    };
    //移除栈顶的元素，同时返回被移出的元素
    this.pop = function(){
        return items.pop();
    };
   //返回栈顶的元素，不对栈做任何修改
    this.peek = function(){
        return items[items.length-1];
    };
    //栈里没元素返回true 有就返回false
    this.isEmpty = function(){
        return items.length == 0;
    };
    //返回栈里的个数
    this.size = function(){
        return items.length;
    };
    //移除栈里所有元素
    this.clear = function(){
        items = [];
    };
    //打印
    this.print = function(){
        console.log(items.toString());
    };
    //数组转字符串
    this.toString = function(){
        return items.toString();
    };
}
```

使用

```
var stack = new Stack(); console.log(stack.isEmpty()); //true
stack.push(5); 
stack.push(8);
console.log(stack.peek());// 8
stack.push(11); 
console.log(stack.size());//3
console.log(stack.isEmpty()); //false
```

##   十进制到二进制

```
function divideBy2(decNumber){

var remStack = new Stack(), 
      rem, 
      binaryString = '';

while (decNumber > 0){ //{1}
     rem = Math.floor(decNumber % 2); //{2}       
     remStack.push(rem); //{3}
     decNumber = Math.floor(decNumber / 2); //{4} 
}

while (!remStack.isEmpty()){ //{5} 
      binaryString += remStack.pop().toString(); 
}

return binaryString;

}
```
使用 （测试时记得把上面封装的Stack函数带上）

```
divideBy2(10);  // "1010"
```

![十转二进制](http://upload-images.jianshu.io/upload_images/5287253-7fb482a908db3fa6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


