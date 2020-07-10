# 函数表达式

```js
function functionName (arg0,arg1) {
	//
}
```

函数声明

```js
var functionName = function(arg0,arg1) {
	//
}
```

区别：函数声明的重要特征就是函数声明提升，意思是再执行代码之前会先赌气函数声明

# 递归

一个函数通过名字调用自身的情况

```js
function factorial(num){
	if (num <= 1){
		return 1;
	} else {
		return num * factorial(num-1);
	}
}
```

下面情况会出错

```js
var anotherFactorial = factorial;
factorial = null;
alert(anotherFactorial(4)); //出错！
```

由于`factorial`不再是函数，调用时会报错

此时，可以使用`arguments.callee`

```js
function factorial(num){
	if (num <= 1){
		return 1;
	} else {
		return num * arguments.callee(num-1);
	}
}
```

`arguments.callee` 是指向正在执行函数的指针，因此可以用它来实现对函数的递归调用

但在严格模式下，不能通过脚本访问`arguments.callee`，可以使用命名函数表达式

```js
var factorial = (function f(num){
	if (num <= 1){
		return 1;
	} else {
		return num * f(num-1);
	}
});
```

# 闭包

## 闭包

**是指有权访问另一个函数作用域中的变量的函数**

常见方式：在一个函数内部创建另一个函数

```js
function createComparisonFunction(propertyName) {
	return function(object1, object2){
		var value1 = object1[propertyName];
		var value2 = object2[propertyName];
		if (value1 < value2){
			return -1;
		} else if (value1 > value2){
			return 1;
		} else {
			return 0;
		}
	};
}
```

当某个函数第一次被调用时，会创建一个执行环境及相应的作用域链，并把作用域链赋值给一个特殊的内部属性。然后，使用this、arguments和其他命名参数的值来初始化函数的活动对象。但在作用域链中，外部函数的活动对象始终处于第二位，外部函数的外部函数的活动对象处于第三位，直至作为作用域链终点的全局执行环境

在函数执行过程中，为读取和写入变量的值，需要在作用域链中查找变量

```js
function compare(value1, value2){
	if (value1 < value2){
		return -1;
	} else if (value1 > value2){
		return 1;
	} else {
		return 0;
	}
}
var result = compare(5, 10);
```



当调用compare()时，会创建一个包含arguments、value1 和value2 的活动对象。全局执行环境的变量对象（包含result和compare）在compare()执行环境的作用域链中则处于第二位。

## 闭包与变量

闭包只能取得包含函数中任何变量的最后一个值

```js
function createFunctions(){
	var result = new Array();
	for (var i=0; i < 10; i++){
		result[i] = function(){
			return i;
		};
	}
	return result;
}
```

这个函数会返回一个函数数组。表面上看，似乎每个函数都应该返自己的索引值，即位置 0 的函数返回 0，位置 1 的函数返回 1，以此类推。但实际上，每个函数都返回 10。因为每个函数的作用域链中都保存着`createFunctions()` 函数的活动对象，所以它们引用的都是同一个变量 i 

我们可以通过创建另一个匿名函数强制让闭包的行为符合预期

```js
function createFunctions(){
	var result = new Array();
	for (var i=0; i < 10; i++){
		result[i] = function(num){
		return function(){
			return num;
			};
		}(i);
	}
	return result;
}
```

在这个版本中，我们定义了一个匿名函数，并将立即执行该匿名函数的结果赋给数组。这里的匿名函数有一个参数 num ，也就是最终的函数要返回的值。在调用每个匿名函数时，我们传入了变量 i 。由于函数参数是**按值传递**的，所以就会将变量 i 的当前值复制给参数 num 。

## this对象

this 对象是在运行时基于函数的执行环境绑定的：在全局函数中， this 等于 window ，而当函数被作为某个对象的方法调用时， this 等于那个对象。

匿名函数的执行环境具有全局性，因此其 this 对象**通常**指向 window。

## 内存泄漏

## 模仿块级作用域

JavaScript没有块级作用域的概念

```js
function outputNumbers(count){
	for (var i=0; i < count; i++){
		alert(i);
	}
	alert(i); //计数
}
```

## 私有变量

严格来讲，JavaScript 中没有私有成员的概念；所有对象属性都是公有的。不过，倒是有一个私有变量的概念。任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。

我们把有权访问私有变量和私有函数的公有方法称为**特权方法**（privileged method）。

```js
function MyObject(){
	//私有变量和私有函数
	var privateVariable = 10;
	function privateFunction(){
		return false;
	}
	//特权方法
	this.publicMethod = function (){
		privateVariable++;
		return privateFunction();
	};
}
```

# 模块模式

道格拉斯所说的模块模式（module pattern）则是为单例创建私有变量和特权方法。

所谓**单例**（singleton），指的就是只有一个实例的对象。按照惯例，JavaScript 是以对象字面量的方式来创建单例对象的。

```js
var singleton = {
	name : value,
	method : function () {
		//这里是方法的代码
	}
};
```

模块模式通过为单例添加私有变量和特权方法能够使其得到增强:

```js
var singleton = function(){
	//私有变量和私有函数
	var privateVariable = 10;
	function privateFunction(){
		return false;
	}
  //特权/公有方法和属性
	return {
		publicProperty: true,
		publicMethod : function(){
			privateVariable++;
			return privateFunction();
		}
	};
}();
```

