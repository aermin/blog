---
title: 面向对象的程序设计(-)
type: tags
date: 2017-04-18 00:11:12
tags:  
      - 学习笔记
      - JS
---

  
  
### 理解对象  
  
#### 定义：对象是无序属性集合，其属性可以包含基本值、对象或者函数  
  
#### 属性类型  
  
* 数据属性  
	* [[Configurabke]]:能否通过delete删除属性从而重新定义属性  
* 访问器属性  
  
### 创建对象  
  
#### 可以通过“构造函数”或者“对象字面量”方法创建函数  
  
* 缺点：通过一个接口创建很多对象，会产生大量的重复代码  
  
#### 工厂模式 
 
<!-- more -->
 
 
``` 
  function creatPerson(name,age,job){  
	    var o=new Object();  
	    o.name=name;  
	    o.age=age;  
	    o.job=job;  
	    return o  
	}                                                                        
  var person1 = creatPerson(“hxvin”,21,”F-E”);  

```
  
* 用函数来封装以特定接口创建对象的细节  
* 缺点：没有解决对象识别的问题  
  
#### 构造函数模式  
  
```
 function Person(name,age,job){  
	    this.name=name;  
	    this.age=age;  
	    this.job=job;  
	    this.sayName=function sayName(){alert(this.name);};  
	};  
	var person1=new Person(“hxvin”,21,"Front-end-Engineer");  
```
  
* 和工厂模式的区别：  
	* 没有显式的创建对象  
	* 直接将属性和方法赋给了this  
	* 没有return语句  
* 用这种方式调用构造函数会经历一下四个步骤  
	* 执行构造函数中的代码（为这个新对象添加属性）  
	* 创建一个新对象  
	* 将构造函数的作用域赋给新对象  
	* 返回新对象  
* 缺点：每个方法都要在每个实例上创建一遍  
	* 上例中sayName方法相当于
	
	> this.sayName=new Funciton("alert(name,age,job)")  

	* 解决方法  

```	 
		  function Person(name,age,job){  
			    this.name=name;  
			    this.age=age;  
			    this.job=job;  
			    this.sayName=sayName  
			};  
			function sayName(){alert(this.name);};  
			//将sayName添加到全局变量中，这样显然有很多不足  
```

  * 原型模式  
  
#### 原型模式  
  
* 概念  
	* 每个函数都有一个prototype（原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以  
		由特定类型的所有实例共享的属性和方法  
  
	* 使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法  
	* 创建原型:

```
	   function Person(){};  
		Person.prototype.name=“hxvin”；  
		Person.prototype.sayname=function(){ alert(this.name);  }；                         
		var person1=new Person();                                   
		person1.dayName(); //“hxvin”  
		
```


* 理解原型  
	* 只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个protoype属性，这个属性指向函数的原型对象。  
	* 在默认情况下，所有原型对象都会自动获得一个constructor（构造函数）属性，包含一个指向prototype属性所在函数的指针  
	* 搜索流程  
		* 当对象实例中与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性会屏蔽原型中的属性  
		* 每当代码读取某个对象属性时，先从实例中搜索，后在原型对象中查找  
		* hasOwnProperty()  
			* 用来检查一个属性时存在于对象实例中还是原型中，这个方法只在给定属性存在于对象实例中时，才会返回true  
			* alert(person1 hasOwnProperty("name"));       //返回true或false  
	* [[prototype]]  
		* 当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型对象  
		* 利用[[prototype]]  
			* ECMAScript5增加了一个新方法，叫Object.getPropertyOf(),在所有支持的实现中，这个方法返回[[prototype]]的值  
				* 支持的浏览器：IE9+,FireFox3.5+,Safari 3.5+,Opera12+,Chrome  
			* 但可以通过isPrototypeOf()方法来确定对象之间是否存在这种关系  
				* alert(Person.protoype isPrototypeOf(person1));   //true  
			* 没有标准的方式访问[[prototype]]  
* 更简单的原型语法 

``` 
	  //可以重新设置constructor  
		function Perspn(){};  
		Person.prototype={  
		    constructor=Person,  
		    name="Nick"  
		};  
  
	  function Person(){};  
		Person.prototype={  
		    name="Nick",  
		    sayName=function(){alert(this.name)}  
		}  
```


	
   * 注意！！！！constructor属性不再指向Person了，我们在这里使用的语法本质上完全重写了默认的prototype对象，因此现在的constructor指向Object构造函数  
* 原型的动态性  
	* 注意：如果把原型修改为另一个对象就等于切断了构造函数与最初原型之间的联系（因为改变了[[prototype]]指针）  
	* 我们对原型对象所做的任何修改都能够立即从实例上反映出来  
* 原型对象的问题（缺点）  
	* 1、省略了为构造函数传递初始化参数这一环节，结果所有实例在默认情况下都将取得相同的属性值  
	* 2、当在实例中对引用类型的值进行操作时，很有可能改变原型中的值。（共享问题）  
		* 由于friends 数组存在于Person.prototype而非person1中，所以刚刚提到的修改也会通过person2. friends (与person2.friends指向同一个数组反映出来  
  
#### 组合使用构造函数模式和原型模式  
 
 ``` 
  function Person(name,age,job){  
	    this.name=name,  
	    this.age=age,  
	    this.job=job  
	};  
	Person.prototype={  
	    constructor=Person,  
	    sayName=function(){alert(this.name)}  
	}  
 ```
 
   * Person1.friends不会影响到Person2.friends，因为它们分别引用了不同的数组  
* 优点  
	* 每个实例都会有自己的一份实例属性副本，但同时由共享着对方法的引用，最大限度的节省了内存  
	* 创建自定义类型最常见的方式  
	* 支持向构造函数传递参数  
  
#### 动态原型模式  
  
```
  function Person(name,age,job){          //属性  
	    this.name=name;  
	    this.age=age;  
	    this.job=job;  
	    //方法  
	    if(typeof sayName != "function"){  
	        Person.prototype.sayName = function(){alert(this.name)}  
	    }  
	} 
	 
```
 
>  
//方法中，只在sayName()方法不存在的清况下，才会将它添加到原型中。 
 这里对原型所做的修改，能够立即在所有实例中得到反映。  

	
* 把所有信息封装在构造函数中，并通过if语句初始化原型  
  
#### 寄生构造函数模式  
 
```
  function Person(name,age,job){  
	    var o = new Object();  
	    o.name = name;  
	    o.age = age;  
	    o.job = job;  
	    o.sayName = function(){alert(o.name)};  
	    return o  
	};  
	var person1 = new Person();  
	
```


* 应用场景：创建有额外方法的特殊对象，而又不想改变其原有的构造函数  
* 特点  
	* 返回的对象和构造函数没有关系  
	* 不能依赖instanceof操作符来确定对象类型  
  
#### 稳妥构造函数模式  
  
* 所谓稳妥对象，指的是没有公共属性， 而且其方法也不引用this的对象。  

```
  function Person(name,age,job){  
	    var o = new Object();  
	    o.sayName = function(){alert(name)}  
	}  
  
	var friend = Person (“hxvin”,”21”,”f-e”);  
		friend.sayName(}; //“hxvin” 

```  

>这样，变蜇person 中保存的是一个稳妥对象， 而除了调用sayName() 方法外,没有别的方式可以访问其数据成员。  
  
* 这种模式创建的对象中，出了使用sayName()方法之外，没有其他任何办法访问name的值。  
* 应用场景： 一些安全的环境中（这些环境中会禁止使用this和new), 或者在防止数据被其他应用程序（如Mashup程序）改动时使用  
* 特点：遵循与寄生构造函数类似的模式，但有两点不同： 一是新创建对象的实例方法不引用this;二是不使用new操作符调用构造函数  
