# DOM扩展

尽管DOM作为API已经非常完善了，但为了实现更多的功能，仍然会有一些标准或专有的扩展

对DOM的两个主要的扩展时Selectors API（选择符API） 和HTML5

# 选择符API

Selectors API是由W3C发起制定的一个标准，致力于让浏览器原生支持CSS查询

## querySelector()方法

接收一个css选择符，返回与该模式匹配的第一个元素，如果没有找到匹配的元素，返回null

```js
var body = documet.querySelector("body")
var myDiv = querySelector('#myDiv')
```



## querySelectorAll()方法

接收参数也是css选择符，返回的时所有匹配的元素，即NodeList实例

## matchesSelector()

接收一个css选择符，检测元素是否与该选择符匹配，返回true或false

# 元素遍历

# HTML5

HTML5规范围绕如何使用新增标记定义了大量Javascript API

## 与类相关的扩充

### getElementByClassName()

### classList属性

操作类名时，需要通过className属性添加、删除和替换类名

HTML新增操作方式，为所有元素添加classList属性，classList属性是新集合类型DOMTokenList的实例。具有length属性，取得每个元素可以使用item()方法或方括号语法，定义了如下方法

1. add()：添加类名
2. contains()：判断是否已有
3. remove()：删除
4. toggle()：已有则删除，没有则添加

## 焦点管理

HTML5添加了辅助管理DOM焦点的功能

首先就是document.activeElement属性，这个属性始终会引用DOM中当前获得了焦点的元素

元素获得焦点的方式有页面加载、用户输入、和在代码中调用focus方法

```js
var button = document.getElementById("myButton")
button.focus()
alert(document.activeElement == button);
```

另外新增了hasFocus()方法，用于确定文档是否获得了焦点

```js
alert(document.hasFocus())
```

## HTMLDocument的变化

### readyState属性

- loading：正在加载文档
- complete：已经加载完文档

```js
if(document.readyState == 'complete') {
	// do sth
}
```

### 兼容模式

自从IE6开始区分渲染页面的模式是标准的还是混杂的，检测页面的兼容模式就成为了浏览器的必要功能

```js
if(document.compatMode == "CSS1Compat") {
    // standard mdoe
}
else {
    // quirks mode
}
```

### head属性

作为对document.body引用文档的<body>元素的补充，HTML5新增了document.head属性

```js
var head = document.head || document.getElementByTagName("head")[0]
```

### 字符集属性

charset

### 自定义数据属性

HTML5规定可以为元素添加非标准的属性，但要添加前缀data-，目的是为元素提供与渲染无关的信息，或者提供语义信息

添加了自定义属性后，可以通过元素的dataset属性来访问自定义属性的值

```js
<div id='myDiv' data-appId='12345' data-myname='Nicolas'></div>
var div = document.getElementById("myDiv")
var appId = div.dataset.appId;
div.dataset.myname = "Michael"
```

### 插入标记

在需要给文档插入大量新HTML标记的情况下，通过DOM操作非常麻烦，因此，使用插入标记的技术，直接擦汗如HTML字符串简单且速度快

#### innerHtml

在读模式下，该属性返回与调用元素的所有子节点对应的HTML标记

在写模式下，innerHtml会根据制定的值创建新的DOM树，然后用这个DOM树完全替换调用元素原先的所有子节点

#### outerHtml

区别：返回调用元素及所有子节点

#### insertAdjacentHtml

接收参数：插入位置和要插入的HTML文本

第一个参数必须为以下值之一：

- beforebegin
- afterbegin
- beforeend
- afterend

#### 内存与性能问题

在删除带有事件处理程序或引用了其他JavaScript对象的子树时，就有可能导致内存占用问题

将元素从文档树中删除后，元素与事件处理程序或JavaScript对象之间的绑定关系在内存中并没有删除

如果这种情况频繁出现，页面占用的内存数量就会明仙增加

因此最好先手动删除

# 专有扩展

未成为标准













