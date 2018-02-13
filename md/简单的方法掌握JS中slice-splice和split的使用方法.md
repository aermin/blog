---
title: '简单的方法掌握JS中slice,splice和split的使用方法'
type: tags
date: 2017-10-15 17:15:26
tags:
		- 开发总结
		- JS
---

首先，js的api的命名一般都有它的意义所在，通过英文就能大概理解是干啥用的。（可见英语多重要。。。）
现在看看slice,splice和split分别是啥意思
* slice：片
* splice： 剪接
* split： 分裂

### slice：片
> 可以理解成是提取某东西的片段

用法1：array.slice(start,end) -> 提取数组的片段

简单可以理解成start是提取片段的开头位置(0开始算)，end是提取的末尾位置(1开始算)
看例子领会一下

 <!-- more -->

```
//如果不传入参数二，那么将从参数一的索引位置开始截取，一直到数组尾
var a=[1,2,3,4,5,6];
var b=a.slice(0,3);  //[1,2,3]
var c=a.slice(3);    //[4,5,6]
 
//如果两个参数中的任何一个是负数，array.length会和它们相加，试图让它们成为非负数，举例说明：
//当只传入一个参数，且是负数时，length会与参数相加，然后再截取
var a=[1,2,3,4,5,6];
var b=a.slice(-1);  //[6]
 
//当只传入一个参数，是负数时,并且参数的绝对值大于数组length时，会截取整个数组
var a=[1,2,3,4,5,6];
var b=a.slice(-6);  //[1,2,3,4,5,6]
var c=a.slice(-8);  //[1,2,3,4,5,6]
 
//当传入两个参数一正一负时，length也会先于负数相加后，再截取
var a=[1,2,3,4,5,6];
var b=a.slice(2,-3);  //[3]
 
//当传入一个参数，大于length时，将返回一个空数组
var a=[1,2,3,4,5,6];
var b=a.slice(6);　　//[]
```

用法2：string.slice(start,end)   -> 提取字符串的片段
道理同上
例子：

```
var a="i am a boy";
var b=a.slice(0,6);  //"i am a"
```
### splice： 剪接

> 可以理解成一个东西剪掉一部分并接上新的部分

用法：array.splice(start,deleteCount,item...)

可以理解成array剪掉初始位置为start，个数为deleteCount的部分，该部分赋值给新的变量，如果有item，则接入被剪掉的部分

例子：

```
var a=['a','b','c'];
var b=a.splice(1,1,'e','f');  //a=['a','e','f','c'],b=['b']
```

### split： 分割

用法：string.split(separator,limit)

> 可以理解成把一个东西从第一位置开始根据separator分割成limit个片段来创建一个字符串数组

例子：

```
var a="0,1,2,3,4,5,6";
var b=a.split("",3); // ["0", ",", "1"]

var a="0,1,2,3,4,5,6";
var b=a.split(",",3);  // ["0", "1", "2"]
```

ps： join的作用恰好与split相反，是添加组装作用

```
var arr = ["0", "1", "2"];
var b = arr.join("");// "012" 
```