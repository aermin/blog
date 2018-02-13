---
title: “js各种遍历总结”
type: tags
date: 2017-11-19 14:44:38
tags:
	- JS
	- 周总结
	
---


最近忙着写项目，是时候来一波总结了。
因为项目中数据交互炒！鸡！多！，遍历很常用，本篇对js的遍历做一次总结


### 一 、普通for循环 (只能遍历数组和字符串)

* 遍历数组

```
var arr = [1,2,3,4,5];
for(var i = 0 ; i < arr.length; i++){
  console.log(i); //1 2 3 4 5 
}
```

不足：重复获取数组长度

> 优化

```
var arr = [1,2,3,4,5];
for(var i = 0 ;  len = arr.length ;  i < len;  i++){
  console.log(i); //1 2 3 4 5 
}
```

使用临时变量，将长度缓存起来，避免重复获取数组长度，当数组较大时优化效果才会比较明显。

这种方法基本上是所有循环遍历方法中性能最高的一种

 <!-- more -->


* 遍历字符串

```
var str = "for循环遍历字符串" ;
for (var i = 0; i < str.length; i++) {
console.log(str[i]+'=='+str.charAt(i)+'=='+str.charCodeAt(i));
//f==f==102
 o==o==111
 r==r==114
循==循==24490
 环==环==29615
 遍==遍==36941
 历==历==21382
 字==字==23383
 符==符==31526
 串==串==20018
}
```

* 遍历对象   不能使用

###二、 for..in..循环 （可以遍历字符串、对象、数组）

这个循环很多人爱用，但实际上，经分析测试，在众多的循环遍历方式中

它的效率是最低的

* 遍历数组

```
var arr = [2,3,4]
for (var i in arr) {
  console.log(arr[i]);//2 3 4 
}
```

* 遍历对象

```
var obj = {name:"xiaoming",
age:"18"
}
for (var prop in obj) {
  console.log(obj[prop]);  // xiaoming  18 
}
```

* 遍历字符串

```
var str  = "循环遍历字符串";
for(var i in str){
  console.log(i+"~"+str[i]);
}
//0~循
1~环
2~遍
3~历
4~字
5~符
6~串
```
### 三、 forEach 循环(不能遍历字符串、对象)

* 遍历数组

```
var arr = [1,2,3,4,5];
arr.forEach(function(e){
	console.log(e); //1 2 3 4 5 
})
```

```
var arr = [3,4,5];
arr.forEach(function(ele,index,arr){
  console.log(ele+'~'+index);  //3 ~ 0    4~1    5~2 
  console.log(arr[index]); // 3 4 5 
});
```
数组自带的foreach循环，使用频率较高，实际上性能比普通for循环弱

*  forEach遍历字符串  不能使用
* forEach遍历对象  不能使用


### 四、 for..of..循环 (需要ES6支持,且不能遍历对象)    

* 遍历数组

```
var str  = [2,3,4,5];
for (var ele of str) {
  console.log(ele); // 2  3  4  5  
}
```
* 遍历对象  不能遍历，需要实现Symbol.interator接口
* 遍历字符串

```
var str  = "循环遍历字符串";
for (var ele of str) {
  console.log(ele);//  循  环  遍  历  字  符  串
}
```

这种方式是es6里面用到的，性能要好于forin，但仍然比不上普通for循环
ps：个人感觉数组的用法和forEach挺相似，都是操作数组的元素

### 五、map 

* 遍历数组

```
function pow(x) {
    return x * x;
}
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);
```
map是操作每一个arr的元素，如上例子中对数组的每一个元素执行pow方法。最后的结果仍是数组。
这种方式也是用的比较广泛的，虽然用起来比较优雅，但实际效率还比不上foreach


![图文无直接关系](http://upload-images.jianshu.io/upload_images/5287253-f4f2848c000b6417.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




> 推荐阅读
[js遍历方式总结](http://www.jianshu.com/p/10bc16564cda)
[JS几种数组遍历方式以及性能分析对比](https://www.cnblogs.com/lvmh/p/6104397.html)