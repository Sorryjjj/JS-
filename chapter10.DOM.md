# 第10章 DOM

**DOM（文档对象模型）是针对 HTML 和 XML 文档的一个 API（应用程序编程接口）。DOM 描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分**。DOM 脱胎于Netscape 及微软公司创始的 DHTML（动态 HTML），但现在它已经成为表现和操作页面标记的真正的跨平台、语言中立的方式。

本章主要讨论与浏览器中的 HTML 页面相关的 DOM1 级的特性和应用，以及 JavaScript 对 DOM1 级的实现。 IE、Firefox、Safari、Chrome 和 Opera 都非常完善地实现了 DOM。

# 节点层次

DOM可以将任何HTML或XML文档描绘成一个由多层节点构成的结构

节点分为几种不同的类型，每种类型分别表示文档种不同的信息或标记

每个节点都有各自的特点、数据和方法

节点之间的关系构成了层次，所有页面标记则表现为一个以特定节点为根节点的树形结构

## Node类型

JavaScript种所有节点类型都继承自Node类型

每个节点都有一个nodeType属性，用于表明节点的类型

节点类型由在Node类型种定义的12个数值常量来表示

- Node.ELEMENT_NODE (1)
- Node.ATTRIBUTE_NODE (2)
- Node.TEXT_NODE (3)
- Node.CDATA_SECTION_NODE (4)
- Node.ENTITY_REFERENCE_NODE (5)
- Node.ENTITY_NODE (6)
- Node.PROCESSING_INSTRUCTION_NODE (7)
- Node.COMMENT_NODE (8)
- Node.DOCUMENT_NODE (9)
- Node.DOCUMENT_TYPE_NODE (10)
- Node.DOCUMENT_FRAGMENT_NODE (11)
- Node.NOTATION_NODE (12)

通过比较常量，可以判断节点的类型

## 节点属性

nodeName：元素的标签名

nodeValue： null

childNodes：保存着一个NodeList对象 -- 类数组对象，用于保存一组有序的节点，可以通过位置访问这些节点，**是基于DOM结构动态执行查询的结果，因此DOM结构的变化能够自动反映在NodeList对象中**

parentNode：指向文档树中的父节点

appendChild：将childNodes列表的末尾添加一个节点

insertBefore：将节点放在childNodes列表种某个特定的位置

replaceChild：替换节点

removeChild：删除节点

cloneNode：拷贝一个节点，参数true为深复制（包括本身及子节点），false为浅赋值（仅赋值自身）

normalize：处理文档树中的文本节点

## Document类型

在浏览器中， document 对象是 HTMLDocument （继承自 Document 类型）的一个实例，表示整个 HTML 页面。而且， document 对象是 window 对象的一个属性，因此可以将其作为全局对象来访问。

通过这个文档对象，不仅可以取得与页面有关的信息，而且还能操作页面的外观及其底层结构。

### 文档的子节点

有两个内置的访问其子节点的快捷方式。第一个就是 documentElement属性，该属性始终指向 HTML 页面中的 ` `元素。

```js
var html = document.documentElement;
// 等价于
// var html = document.childNodes[0];
// var html = document.firstChild;
```



document 对象还有一个 body 属性，直接指向 ``元素。因为开发人员经常要使用这个元素，所以 document.body 在 JavaScript代码中出现的频率非常高。

```js
var body = document.body;
```

Document 另一个可能的子节点是 DocumentType 。通常将 <!DOCTYPE> 标签看成一个与文档其他部分不同的实体，可以通过 doctype 属性（在浏览器中是 document.doctype ）来访问它的信息。

```js
// 不同浏览器中有差异
var doctype = document.doctype;
```

### 文档信息

作为 HTMLDocument 的一个实例， document 对象还有一些标准的 Document 对象所没有的属性。其中第一个属性就是 title ，包含着元素中的文本。

```js
// 文档标题
var title = document.title;
// 页面完整的 URL
var url = document.URL;
// 页面的域名
var domain = document.domain;
// 接到当前页面的那个页面的 URL
var referer = document.referer;
```

### 查找元素

说到最常见的 DOM 应用，恐怕就要数取得特定的某个或某组元素的引用，然后再执行一些操作了。取得元素的操作可以使用 document 对象的几个方法来完成。其中， Document 类型为此提供了两个方法： getElementById() 和getElementsByTagName() 。

### 文档写入

有一个 document 对象的功能已经存在很多年了，那就是将输出流写入到网页中的能力。这个能力体现在下列 4 个方法中： write() 、 writeln() 、 open() 和 close() 。其中， write() 和 writeln()方法都接受一个字符串参数，即要写入到输出流中的文本。 write() 会原样写入，而 writeln() 则会在字符串的末尾添加一个换行符（ \n ）。

## Element类型

用于表现XML或HTML元素，提供了对元素标签名、子节点、及特性的访问

nodeType：1

nodeName：元素标签名（tagName）

### HTML元素

所有HTML元素都有HTMLElement类型表示，继承自Element

- id：元素在文档中的唯一标识符
- title：有关元素的附加说明信息，一般通过工具提示条显示出来
- lang：语言
- className：与元素的class特性对应，即为元素指定的css类
- dir：语言的方向

```html
<div>
    id='myDiv' class='bd' title-'Body Text' lang='en' dir='ltr'
</div>
var div = document.getElementById('myDiv');
```

### 取得特性

每个元素都有一个或多个特性，这些特性的用途是给出相应元素或内容的附加信息

操作特性的DOM方法主要有三个

- getAttribute()
- setAttribute()
- removeAttribute()

这三个方法可以针对任何特性使用

根据HTML5规范，自定义特性应该加上data-前缀以便验证

### 设置特性

### attributes属性

Element类型是使用attributes属性的唯一一个DOM节点类型

attributes属性中包含一个NamedNodeMap，与NodeList类似，也是一个动态的集合

元素的每一个特性都由一个Attr节点表示，每个节点都保存在NameNodeMap对象中

NameNodeMap对象拥有下列方法

- getNamedItem(name)
- removeNamedItem(name)
- setnamedItem(node)
- item(pos)：返回位于数字pos位置的节点

### 创建元素

```js
var div = document.creatElement('div')
div.id = 'myNewDiv'// 仅设置特性，新元素未被添加到文档树中
document.body.appendChild(div)
// or replaceChild() insertBefore()
```

### 元素的子节点

childNodes属性包含了元素的所有子节点，这些子节点可能是元素、文本节点、注释或处理指令

## Text类型

文本节点由Text类型表示

包含的是可以照字面解释的纯文本内容

纯文本中可以包含转义后的HTML字符，但是不能包含HTML代码

- nodeType：3
- nodeName：'#text'
- nodeValue: 文本
- 不支持子节点
- parentNode是一个Element
- appendData(text)

##  Comment类型

注释在DOM中是通过Comment类型表示的

- nodeType:8
- nodeName: '#comment'
- nodeValue: 注释的值
- 不支持子节点
- parentNode可能是document或Element

与Text类型继承自相同的基类

## Attr类型

元素的特性在DOM中以Attr类型表示

从技术角度，特性就是存在于元素的attributes属性中的节点

- nodeType: 11
- nodeName: 特性的名称 
- nodeValue: 特性的值
- parentNode: null
- HTML中不支持子节点

# DOM操作技术

## 动态脚本

创建动态脚本的两种方式

- 插入外部文件
- 直接插入JavaScript代码

## 动态样式

## 操作表格









