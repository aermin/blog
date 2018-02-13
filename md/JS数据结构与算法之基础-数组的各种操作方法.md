---
title: JS数据结构与算法之基础--数组的各种操作方法
date: 2017-12-19 12:57:34
tags:
	- 数据结构
	- 算法
	- JS
---

### 先来个经典数组操作题：
求斐波那契数列的前20 个数字。已知斐波那契数列中第一个数字是1 ，
第二个是2 ，从第三项开始，每一项都等于前两项之和

```
var fibonacci = []; 
fibonacci[0] = 1;
fibonacci[1] = 2; 
for(var i = 2; i < 20; i++){
fibonacci[i] = fibonacci[i-1] + fibonacci[i-2]; 
}
console.log(fibonacci); //[1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946]
```
### 数组增删 
>  push  unshift shift  pop

```
var numbers = [0,1,2,3,4,5,6,7,8,9];
numbers[numbers.length] = 10; //插入尾
numbers.push(11);//插入尾
numbers.push(12, 13); //插入尾
numbers.unshift(-2); //插入头
numbers.unshift(-4, -3);//插入头
numbers.shift();//删除第一个数
numbers.pop(); // 删除最后一个数
console.log(numbers);  //[-3, -2, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12,]
```
通过push和pop方法，就能用数组来模拟栈
通过shift和unshift方法，就能用数组模拟基本的队列数据结构

<!-- more -->

* 二维数组

```
var averageTemp = [];
averageTemp[0] = [72,75,79,79,81,81];
averageTemp[1] = [81,79,75,75,73,72];

function printMatrix(myMatrix) {
for (var i=0; i<myMatrix.length; i++){
for (var j=0; j<myMatrix[i].length; j++){
console.log(myMatrix[i][j]);
}
}
}

printMatrix(averageTemp); //72 75 79 79 81 81 81 79 75 75 73 72
```
三维就三个for循环，以此类推。

### 数组迭代方法 

![数组迭代方法](http://upload-images.jianshu.io/upload_images/5287253-f169bd23edddb96a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![数组迭代方法](http://upload-images.jianshu.io/upload_images/5287253-0e978239c71a83a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*  数组合并

```
var zero = 0;
var positiveNumbers = [1,2,3];
var negativeNumbers = [-3,-2,-1];

console.log(negativeNumbers.concat(zero, positiveNumbers))//-3 -2 -1 0 1 2 3
console.log(negativeNumbers.concat(positiveNumbers))//-3 -2 -1 1 2 3

```
* 迭代器函数


```
var isEven = function (x) {
// 如果x是2的倍数，就返回true
console.log(x);
return (x % 2 == 0) ? true : false;
// 也可以写成return (x % 2 == 0) ? true : false
};
var numbers = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15];
```

> [every]数组numbers的第一个元素是1，它不是2的倍数， 因此isEven 函数返回false，然后every执行结束。

```
numbers.every(isEven); // 1 false  
```

> [some] some方法和every的行为类似，不过some方法会迭代数组的每个元素，直到函数返回true：

```
numbers.some(isEven); //1 2 true
```

> [forEach]如果要迭代整个数组，可以用forEach方法。它和使用for循环的结果相同：

```
numbers.forEach(function(x){
console.log((x % 2 == 0));
}); // [false, true, false, true, false, true, false, true,
false, true, false, true, false, true, false]
```
>  [map]JavaScript还有两个会返回新数组的遍历方法。第一个是map：

```
var myMap = numbers.map(isEven); //：[false, true, false, true, false, true, false, true,false, true, false, true, false, true, false]
 ```
> [filter]。它返回的新数组由使函数返回true的元素组成：

```
var evenNumbers = numbers.filter(isEven);//[2, 4, 6, 8, 10, 12, 14]
```
> [reduce]reduce方法接收一个函数作为参数，这个函数有四个参数：previousValue、currentValue、index和array。这个函数会返回一个将被叠加到累加器的值，reduce方法停止执行后会返回这个累加器。对一个数组中的所有元素求和:

```
numbers.reduce(function(previous, current, index){
return previous + current;
}); //120
```

还有我写的另外一篇 [js各种遍历方法总结](http://www.jianshu.com/p/a32d527df6df)
