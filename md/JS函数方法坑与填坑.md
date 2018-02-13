---
title: JS函数方法坑与填坑
type: tags
date: 2017-05-17 13:37:50
tags:
      - 学习笔记
      - JS
---
## 首先，先认识下什么叫函数的方法：
在一个对象中绑定函数，称为这个对象的方法。
### 再给具体的例子：
```
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 27
```

xiaoming是一个对象，age（）函数就是该对象的方法。
此时this指向xiaoming

<!-- more -->

### 如果拆开写：
```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 27, 正常结果
getAge(); // NaN
var fn = xiaoming.age; // 先拿到xiaoming的age函数
fn(); // NaN

```

以对象的方法形式调用没问题，该函数的this指向被调用的对象xiaomign； 你会发现单独调用getAge()函数返回NaN,该函数的this指向全局对象，也就是window，这是JS的一个`坑`。

### 填坑：

#### 1. 要保证this指向正确，必须用obj.xxx()的形式调用！

这里需`注意`：obj.xxx()也不是什么情况下都适用的，比如在在函数内定义的函数：

```
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - this.birth;
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // Uncaught TypeError: Cannot read property 'birth' of undefined
填坑：
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var that = this; // 在方法内部一开始就捕获this
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth; // 用that而不是this
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // 27
```
#### 2.或者用apply()和call()方法修复函数调用：

函数本身的apply方法，它接收两个参数，第一个参数就是需要绑定的this变量，第二个参数是Array，表示函数本身的参数。

```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空

```
apply()类似的方法是call()，唯一区别是：

apply()把参数打包成Array再传入；

call()把参数按顺序传入。
如：
```
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

