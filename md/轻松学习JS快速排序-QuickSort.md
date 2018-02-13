---
title: 轻松学习JS快速排序(QuickSort)
type: tags
date: 2017-04-29 11:32:03
tags:
      - 学习笔记
      - JS
---
### 需了解的基础知识
 
 1.递归函数：编程语言中，函数Func(Type a,……)直接或间接调用函数本身，则该函数称为递归函数。
 ` 一个典型阶乘递归函数：`
 ```
 function fact(num){ 
    if (num<=1){ 
    return 1; 
    }else{ 
    return num*fact(num-1); 
 } 
 } 
 ```
 ` 该函数的弊端：`
 ```
 var another=factorical;
 factorical=null;
 console.log(another(2))//会报错说 factorical not a function
 ```
 ` 解决方法`: 用arguments.callee去代替函数名，就可以确保函数不管怎么调用都不会出错。
 
 <!-- more -->
 
 ```
 function factorical(num){
　　if(num<=1){
　　　　return 1;
　　}
　　else{
　　　　return num*arguments.callee(num-1);
　　}
 }
 var another=factorical;
 factorical=null;
 console.log(another(2))//2
 ```
 (来自js高程)
 2.[JavaScript中的splice方法用法详解](http://www.jb51.net/article/88894.htm)
 3.[JavaScript concat()方法](http://www.w3school.com.cn/jsref/jsref_concat_array.asp)
 3.[Javascript之Math对象详解](http://www.jb51.net/article/86083.htm)

### 步骤
以下内容整理自`阮一峰老师`的[快速排序（Quicksort）的Javascript实现](http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html)

> 首先，定义一个quickSort函数，它的参数是一个数组。

var quickSort = function(arr) { 

> 然后，检查数组的元素个数，如果小于等于1，就返回。

　　if (arr.length <= 1) { return arr; } 
　　
> 接着，选择"基准"（pivot），并将其与原数组分离，再定义两个空数组，用来存放一左一右的两个子集。

　　var pivotIndex = Math.floor(arr.length / 2);
　　var pivot = arr.splice(pivotIndex, 1);
　　var left = [];
　　var right = [];
　　
> 然后，开始遍历数组，小于"基准"的元素放入左边的子集，大于基准的元素放入右边的子集。

　　for (var i = 0; i < arr.length; i++){
　　　　if (arr[i] < pivot) {
　　　　　　left.push(arr[i]);
　　　　} else {
　　　　　　right.push(arr[i]);
　　　　}
　　}
　　
> 最后，使用递归不断重复这个过程，就可以得到排序后的数组。

　　return quickSort(left).concat([pivot], quickSort(right));
};

> 使用的时候，直接调用quickSort()就行了。

### 最终的快排函数

```
var quickSort = function(arr) {
　　if (arr.length <= 1) { return arr; }
　　var pivotIndex = Math.floor(arr.length / 2);
　　var pivot = arr.splice(pivotIndex, 1);
　　var left = [];
　　var right = [];
　　for (var i = 0; i < arr.length; i++){
　　　　if (arr[i] < pivot) {
　　　　　　left.push(arr[i]);
　　　　} else {
　　　　　　right.push(arr[i]);
　　　　}
　　}
　　return quickSort(left).concat([pivot], quickSort(right));
};
```

### 实例运用

```
var array = [7,4,1,9,6,3,2,5,8] ;
quickSort(array); //输出1，2，3，4，5，6，7，8，9
```

