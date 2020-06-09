# 引用类型

引用类型的值（对象）是引用类型的一个实例

在ES中，引用类型是一种数据结构，用于将数据和功能组织在一起

引用类型有时候也被称为**对象定义**，因为它们描述的是一类对象所具有的属性和方法

对象是某个特定引用类型的实例，新对象是使用new操作符后跟一个构造函数来创建的

常见的引用类型有`Object` `Array` `Date` `RegExp` `Function`等。

# Object类型

## 构造函数

```js
var person = new Object ();
person.name = 'Ni'
```

## 对象字面量

```js
var person = {
	name: 'jack',
    age: 20
}

var person1 = {};
person1.name = 'Tom';
person1.age = 30;

```

## 访问对象属性

### 点表示法

一般来说，访问对象属性时使用的都是点表示法，这也是很多面向对象语言种通用的语法

```js
alert(person.name)
```

### 方括号表示法

将要访问的属性以字符串的形式放在方括号种

```js
alert(person["name"])
```

从功能上看，两种方法没有区别，但方括号的优点是可以通过变量来访问属性

如果属性名种报班会导致语法错误的字符，或者属性名使用的是关键字或保留字，也可以使用方括号法

```js
var propertyname = "name"
alert(person[propertyname])

person["first name"] = "Jerry"
```



# Array类型

除了Object 之外，Array 类型恐怕是ES中最常用的类型了

ES数组的每一项可以保存任何类型的数据

## 构造函数

```js
var colors = new Array(3);
var names = new Array("Greg");
//new可省略
var colors = Array(3);
```

## 对象字面量

```js
var colors = ["red", "blue", "green"];
var names = [];

```

[深入 JavaScript 数组：进化与性能](http://www.wemlion.com/post/javascript-array-evolution-performance/)

## 检测数组

```js
alert(Array.isArray(colores))
```

## 转换方法

所有对象都具有toLocaleString()、toString()和valueOf()方法。其中，调用数组的toString()方法会返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。

### toString

返回数组的字符串表示，每个值的字符串表示拼接成了一个字符串

### valueOf

返回指定对象的原始值

### toLocaleString

用于返回格式化对象后的字符串，该字符串格式因不同语言而不同

在没有指定区域的基本使用时，返回使用默认的语言环境和默认选项格式化的字符串。

当调用数组的toLocaleString()方法时，它也会创建一个数组值的以逗号分隔的字符串。而与前两个方法唯一的不同之处在于，这一次为了取得每一项的值，调**用的是每一项的toLocaleString()方法**，而不是toString()方法

### join

使用不同分隔符构建字符串

```js
var colors = ["red", "green", "blue"];
alert(colors.join(",")); //red,green,blue
alert(colors.join("||")); //red||green||blue
```

## 栈方法

push()方法可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后**数组的长度**。

pop()方法则从数组末尾移除最后一项，减少数组的length 值，然后返回**移除的项**。

```js
var colors = new Array(); // 创建一个数组
var count = colors.push("red", "green"); // 推入两项
alert(count); //2
count = colors.push("black"); // 推入另一项
alert(count); //3
var item = colors.pop(); // 取得最后一项
alert(item); //"black"
alert(colors.length); //2
```

## 队列

数组方法shift()，它能够移除数组中的第一个项并返回该项，同时将数组长度减1

```js
var colors = new Array(); //创建一个数组
var count = colors.push("red", "green"); //推入两项
alert(count); //2
count = colors.push("black"); //推入另一项
alert(count); //3
var item = colors.shift(); //取得第一项
alert(item); //"red"
alert(colors.length); //2
```

unshift()方法数组前端添加任意个项并返回新数组的长度

```js
var colors = new Array(); //创建一个数组
var count = colors.unshift("red", "green"); //推入两项
alert(count); //2
count = colors.unshift("black"); //推入另一项
alert(count); //3
var item = colors.pop(); //取得最后一项
alert(item); //"green"
alert(colors.length); //2
```

