# 属性类型

## 数据属性

数据属性包含一个数据值的位置，可以读取和写入值

具有4个描述其行为的**特性**

- [[Configurable]]：表示能否通过delete删除属性从而重新定义属性，默认为true
- [[Enumerable]]：表示能否通过for-in循环返回属性，默认为true
- [[Writable]]：表示能否修改属性的值，默认为true
- [[Value]]：包含这个属性的数据值，从这里读取和写入，默认为undefined

```js
var person = {
    name: "Nicholas"
}
```

这里创建了一个名为name的属性，[[Value]]特性将被设置为"Nicholas"

要修改属性默认的特性，必须使用```Object.defineProperty()```方法

```js
var person = {}
Object.defineProperty(person,"name",{
    writable: false,
    value: "Nicholas"
})
```

这里为person创建了一个name属性，值为Nicholas，不可修改

非严格模式下，赋值操作会被忽略，严格模式下，会抛出错误

## 访问器属性

访问器属性不包含数据值

它们包含一堆getter和setter函数

在读取访问器属性时，会调用getter函数，负责返回有效的值

在写入访问器属性时，会调用setter函数并传入新值，这个函数负责决定如何处理函数

访问器属性有4个特性

- [[Configurable]]：表示能否通过delete删除属性从而重新定义属性，默认为true
- [[Enumerable]]：表示能否通过for-in循环返回属性，默认为true
- [[Get]]：读取属性时调用的函数，默认为undefined
- [[Set]]：写入属性时调用的函数，默认为undefined

访问器属性不能直接定义，必须使用```Object.defineProperty()```方法

```js
var book = {
    _year: 2004,
    edition: 1
}
Object.defineProperty(book,"_year",{
    get: function () {
		return this._year
    },
    set:function (newValue) {
        if(newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
        }
    }
})

```

# 创建对象

## 工厂模式

定义：**定义一个工厂类，根据传入的参数不同返回不同的实例，被创建的实例具有共同的父类或接口。**

在ES中无法创建类，开发人员就发明了一种函数，用函数来封装以特定接口创建对象的细节

```js
function createPerson(name,age,job) {
   var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function () {
		alert(this.name)
    }
    return o;
}
```

根据传入的参数来构建一个包含所有必要信息的Person对象。可以无数次调用这个函数，每次都会返回一个包含三个属性一个方法的对象。

**工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即如何知道一个对象的类型）**

## 构造函数模式

ES中的构造函数可以用来创建特定类型的对象，像Object和Array这样的原生构造函数，在运行时会自动出现在执行环境中。

**因此，可以创建自定义的构造函数，从而定义自定义对象类型的属性和方法。**

```js
function Person (name,age,job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function () {
        alert(this.name)
    }
}

var person1 = new Person("Nicholas",29,"Software Engineer");
var person2 = new Person("Greg",27,"Doctor");
```

`person1`和`person2`分别保存着`Person`的一个不同的实例，这两个对象都有一个`constructor`（构造函数）属性，指向Person

```js
alert(person1.constructor == Person) //true
```

**创建自定义构造函数可以将对象实例标识为一种特定的类型，弥补了工厂模式的缺点。**

**使用构造函数的问题，就是每个方法都要在每个实例上创建一遍**

## 原型模式

```js
function Person {  
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Doctor";
Person.prototype.sayName = function() {
    alert(this.name);
}
var person1 = new Person();
var person2 = new Person();
person.sayName();
```

与构造函数模式不同的是，新对象的这些属性和方法是由所有实例共享的。换句话说，person1 和person2 访问的都是同一组属性和同一个sayName()函数。

### 理解原型对象

无论什么时候，只要创建了一个新函数，就会自动为该函数创建一个prototype属性，这个属性指向函数的原型对象。

默认情况下，所有原型对象都会自动获得一个constructor属性，指向prototype所在函数的指针

![img](https://camo.githubusercontent.com/3854356bfc76bae034278541cf5b1c5e005fc7c3/68747470733a2f2f73696e61636c6f75642e6e65742f70726f2d6a732f362d312e706e67)



可以通过`isPrototypeOf()`方法来确定对象之间是否存在原型关系。

`Object.getPrototypeOf()`返回[[Prototype]]的值

使用`hasOwnProperty()`方法可以检测一个属性是存在于实例中，还是存在于原型中





