---
title: 关于this对象的一个易错点
type: tags
date: 2017-04-26 23:39:31
tags:
      - 学习笔记
      - JS
---
#### 先给一个例子

```
var name = "The Window"
var object = {
    name : "My Object",
    
    f1 : function(){
       return function(){
           return this.name;
        };
    }
 };

  alert(object.f1()());  
       
```
<!-- more -->

> 这个例子返回的字符串是 "The Window"，为啥不是"My Object"呢？

* 原因是每个函数在被调用时， 其活动对象都会自动取得两个特殊变量：`this`和`arguments`</strong>。内部函数在搜索这两个变最时， 只会搜索到其活动对象为止，因此永远不可能直接访问外部函数中的这两个变量。
* 也就是说要想让闭包【return function(){}】访问到外部函数【f1 :function(){}】里的this（或者arguments）变量，就要先将其赋值给到一个闭包能够访问到的变量里，如var that = this，这样就可以了。

#### 修改后的例子如下

```
var name = "The Window"
var object = {
    name : "My Object",
    
    f1 : function(){
       var that = this; //添加这行
       return function(){
           return this.name;
        };
    }
 };

  alert(object.f1()()); 

```
> 返回"My Object"

#### 另外一个例子

```
var name = "The Window•;
var object = {
    name : "My Object";
    getName: function() {
         return this.name;
    }
 };
```
> 上一个例子中的
return function(){
    return this.name；
}  
被换掉了，不再是闭包

```
object.getName(); //'My Object'
(object.getName)(); //'My Object"
(object.getName = object.getNamel (); //"The Window•, 在非严格模式下
```
> 第一行代码跟平常一样词用了object.getName (), 返回的是飞y Object", 因为this.name
就是object.name。第二行代码在调用这个方法前先给它加上了括号。虽然加上括号之后， 就好􀉀只
是在引用一个函数， 但this的值得到了维持， 因为objec七.getName 和(object.getName)的定义
是相同的。第三行代码先执行了一条赋值语句，然后再诮用赋值后的结果。因为这个赋值表达式的值是
函数本身， 所以this的值不能得到维持， 结果就返回了崎The Window飞