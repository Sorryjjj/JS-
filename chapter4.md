# 数据类型

## 分类

1、基本类型（值类型）

指简单的数据段

- String：任意字符串
- Number：任意数字
- boolean：true/false
- undefined：undefined
- null：null

2、对象（引用）类型

可能由多个值构成的对象

- Object：任意对象
- Function：函数（可执行的对象）
- Array：数组（有序下标）

## 复制变量值

### 复制基本类型

会在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上

两个变量可以参与任何操作不会相互影响

### 复制引用类型

当从一个变量向另一个变量复制引用类型的值时，同时也会将存储在变量对象中的值复制一份放到位新变量分配的空间中。实际上，这个值的副本是一个指针，指向存储在堆中的一个对象。

复制操作之后，两个变量实际上引用同一个对象，改变其中一个变量，将会影响另一个变量

## 传递参数

ES中所有函数的参数都是按值传递的

即，把函数外部的值复制给函数内部参数，和变量复制一样

## 检测类型

- typeof
- instanceof

typeof操作符石确定一个变量是字符串、数值、bool值还是undefined的最佳方法

如果变量是null或者对象，则typeof都会返回Object

如果变量是引用类型，那么instanceof将会返回trus

```js
var person = {
	name: 'haha'
}
alert(person instanceof Object)
alert(person instanceof Array)
alert(person instanceof RegExp)
```

规定：所有引用类型的值都是Object的实例

instanceof检测基本类型始终返回false

## question

1. undefined与null的区别：未赋值/定义并赋值，值为null
2. 什么时候赋值为null：初始赋值，表明将要赋值为对象 / 结束前。解除引用，回收垃圾



## 执行环境及作用域













