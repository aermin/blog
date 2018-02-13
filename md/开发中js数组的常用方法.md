---
title: 开发中js数组的常用方法
type: tags
date: 2017-04-27 13:03:14
tags:
      - 学习笔记
      - JS
---


* 在ES5中，一共有9个Array方法：

Array.prototype.indexOf;
Array.prototype.lastIndexOf;
Array.prototype.every;
Array.prototype.some;
Array.prototype.forEach;
Array.prototype.map;
Array.prototype.filter;
Array.prototype.reduce;
Array.prototype.reduceRight;

5种比较常用=>index(),filter(),forEach(),map(),reduce().
> ps:reduce()还没搞懂，就先不整理了。
文末有个实例了解实际运用。

<!-- more -->

### 介绍

#### 1) indexOf
indexOf()方法返回在该数组中第一个找到的元素位置，如果它不存在则返回-1。
不使用indexOf时：

```
var arr = ['a','b','c'],
found = false;
 
for(var i= 0, l = arr.length; i< l; i++){
if(arr[i] === 'a'){
found = true;
}
}
console.log("found:",found);//输出found: true
```

* indexOf使用后

```
var arr = ['a','b','c'];
 
console.log("found:", arr.indexOf("a") != -1);//输出found: true
```
#### 2) filter
该filter()方法创建一个新的匹配过滤条件的数组。

* 不用 filter() 时

```
var arr = [
  ["a", 2],
  ["b", 5],
  ["c", 3],
  ["b", 16],
];
   
var newArr = [];
 
for(var i= 0, l = arr.length; i< l; i++){
  if(arr[i][0] == "b" ){
newArr.push(arr[i]);
}
}
 
alert(newArr);//输出b,5,b,16
```

* 用了 filter():

```
var arr = [
  ["a", 2],
  ["b", 5],
  ["c", 3],
  ["b", 16],
];
  var newArr = arr.filter(function(element, index, array){
  return element[0] == "b" ;
});
 
alert(newArr); //输出b,5,b,16
```

#### 3) forEach()
forEach为每个元素执行对应的方法,用来替换for循环

* 不用 forEach() 时

```
var arr = [1,2,3,4,5,6,7,8];
for(var i= 0, l = arr.length; i< l; i++){
console.log(arr[i]); //输出 1 2 3 4 5 6 7 8 
}
```

* 用了forEach()

```
var arr = [1,2,3,4,5,6,7,8];
arr.forEach(function(element, index, array){
console.log(element);//输出 1 2 3 4 5 6 7 8 
});
```

#### 4) map()
map()对数组的每个元素进行一定操作（映射）后，会返回一个新的数组，
不使用map
map()是处理服务器返回数据时是一个非常实用的函数。

* 不用 map() 时

```
var oldArr = [{first_name:"Colin",last_name:"Toh"},{first_name:"Addy",last_name:"Osmani"},{first_name:"Yehuda",last_name:"Katz"}];
 
function getNewArr(){
   
  var newArr = [];
   
  for(var i= 0, l = oldArr.length; i< l; i++){
    var item = oldArr[i];
    full_name = [item.first_name,item.last_name].join(" ");
    newArr[i] = full_name;
  }
   
  return newArr;
}
 
console.log(getNewArr());//输出 ["Colin Toh", "Addy Osmani", "Yehuda Katz"]

```

* 使用map后

```
var oldArr = [{first_name:"Colin",last_name:"Toh"},{first_name:"Addy",last_name:"Osmani"},{first_name:"Yehuda",last_name:"Katz"}];
 
function getNewArr(){
     
  return oldArr.map(function(item,index){
    full_name = [item.first_name,item.last_name].join(" ");
    return full_name;
  });
   
}
 
console.log(getNewArr());  //输出 ["Colin Toh", "Addy Osmani", "Yehuda Katz"]
```

### 百度IFE的js任务(二)的运用
[传送门](http://ife.baidu.com/course/detail/id/91)
任务描述:参考以下示例代码，页面加载后，将提供的空气质量数据数组，按照某种逻辑（比如空气质量大于60）进行过滤筛选，最后将符合条件的数据按照一定的格式要求显示在网页上

实现思路：
1、用filter()方法筛选出值大于60的城市赋值给一个新的数组。
2、用sort()对这个新的数组进行由大到小的排序。
3、用forEach()代替for循环并动态创建li标签并打印名次和城市及其空气质量值



```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>IFE JavaScript Task 01</title>
  </head>
<body>

  <h3>污染城市列表</h3>
  <ul id="aqi-list">
<!--   
    <li>第一名：福州（样例），10</li>
      <li>第二名：福州（样例），10</li> -->
  </ul>

<script type="text/javascript">

var aqiData = [
  ["北京", 90],
  ["上海", 50],
  ["福州", 10],
  ["广州", 50],
  ["成都", 90],
  ["西安", 100]
];

(function () {

  /*
  在注释下方编写代码
  遍历读取aqiData中各个城市的数据
  将空气质量指数大于60的城市显示到aqi-list的列表中
  */
   var aqiul=document.getElementById('aqi-list'); //获取数组

 //用filter()方法筛选出空气质量指数大于60的数组
   var filtered = aqiData.filter(function(element, index, array){
      return (element[1] >= 60);
    }) ;

   filtered.sort(function(a,b){ //从大到小排序
      return b[1]-a[1];
  });
   
  //    (function wirte(){    //输出  用for循环
  //   for(var i=0;i<filtered.length;i++){
  //     var li=document.createElement('li');
  //     aqiul.append(li);
  //     li.innerHTML="第"+(i+1)+"名："+filtered[i];
  //   }
  // })();

//用forEach方法代替for循环
 filtered.forEach(function(element, index, array){
      var li=document.createElement('li');
         aqiul.append(li);
         li.innerHTML="第"+(index+1)+"名："+filtered[index];
 });

})();

</script>
</body>
</html>
```