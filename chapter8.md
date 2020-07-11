BOM

# window对象

BOM的核心对象时window，表示浏览器的一个实例

既是JavaScript访问浏览器窗口的一个接口，又是ECMAScript规定的global对象

## 全局作用域

所有在全局作用域中声明的变量、函数都会编程window对象的属性和方法

全局变量不能通过delete操作符删除，而直接定义在window对象上的属性可以

## 窗口关系和框架

# location对象

提供与当前窗口中加载的文档有关的信息，还提供了一些导航功能

既是window对象的属性，也是document对象的属性

 将url解析为独立的片段，保存在不同属性中，如host（服务器名称和端口）、hostname（服务器名称）、href（当前加载页面的完整url）等

## 查询字符串参数

```js
function getQueryStringArgs(){
    //取得查询字符串并去掉开头的问号
    var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
    //保存数据的对象
    args = {},
    //取得每一项
    items = qs.length ? qs.split("&") : [],
    item = null,
    name = null,
    value = null,
	//在 for 循环中使用
    i = 0,
    len = items.length;
    //逐个将每一项添加到 args 对象中
    for (i=0; i < len; i++){
        item = items[i].split("=");
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);
		if (name.length) {
			args[name] = value;
		}
	}
	return args;
}

//假设查询字符串是?q=javascript&num=10
var args = getQueryStringArgs();
alert(args["q"]); //"javascript"
alert(args["num"]); //"10"
```

## 位置操作

location.assign()

location.href = '';

## navigator

识别客户端浏览器

检测插件

注册处理程序

# screen对象

表明客户端的能力，包括浏览器窗口外部的显示器信息等，每个浏览器中的screen对象都包含着各不相同的属性

# history对象

保存用户上网的历史纪录

```js
history.go(-1)

history.back();

history.forward();

if(history.length > 0) {
    
}
```



