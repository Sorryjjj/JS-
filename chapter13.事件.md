# 事件

主要内容：

- 事件流
- 事件处理程序
- 事件类型

**JavaScript与HTML之间的交互是通过事件实现的**

事件，就是文档或浏览器窗口中发生的一些特定的交互瞬间

可以使用侦听器（或处理程序）来预定事件，以便事件发生时执行相应的代码，这种在传统软件工程中被称为观察员模式的模型，支持页面的行为（JavaScript代码）与页面（HTML和CSS代码）的外观之间的松散耦合

# 事件流

**事件流描述的是从页面中接收事件顺序**，IE与NetScape开发团队提出了差不多是完全相反的事件流的概念

IE的事件流是**事件冒泡流**

Netscape的事件流是**事件捕获流**

## 事件冒泡

事件开始时**由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）**

如果单击了页面中的div元素，那么click事件会按照如下顺序传播

1. div
2. body
3. html
4. document

![image.png](https://i.loli.net/2020/07/23/bVD8z2uJUixBvso.png)

首先在div元素上发生，即我们单击的元素，然后click事件沿DOM树向上传播，在每一级节点上都会发生，直至传播到document对象

## 事件捕获

事件捕获的思想是**不太具体的节点应该更早接收到事件，最具体的节点应该最后接收到事件**

事件捕获的用意在于**在事件到达预定目标之前捕获它**

如果单击了页面中的div元素，那么click事件会按照如下顺序传播

1. document
2. html
3. body
4. div

![image.png](https://i.loli.net/2020/07/23/AJFa7ZPsqTWeBjo.png)

在事件捕获过程中，document对象首先接收到click事件，然后事件沿DOM树依次向下，一直传播到事件的实际目标div元素

由于兼容性问题，建议使用事件冒泡

## DOM事件流

“DOM2级事件”规定的事件包括三个阶段：

1. 事件捕获阶段：为捕获事件提供机会
2. 处于目标阶段：实际的目标接收到事件
3. 事件冒泡阶段：在这个阶段对事件做出响应

在DOM事件流中，实际的目标（div元素）在捕获阶段不会接收到事件，即在捕获阶段到达body后就停止了，进入处于目标阶段，于是事件在div上发生，并在事件处理中被看成冒泡阶段的一部分。然后冒泡阶段发生，事件又传回文档



![image.png](https://i.loli.net/2020/07/23/LoXFWeHs4MrQ1hK.png)

# 事件处理程序

事件就是用户或浏览器自身执行的某种动作，例如click、load、mouseover，都是事件的名字

响应某个事件的函数就叫做事件处理程序（或事件侦听器）

事件处理程序的名字以on开头，如onclick

## HTML事件处理程序

某个元素支持的每种事件，都可以使用一个与相应事件处理程序同名的HTML特性指定（写在HTML中的函数）

这个特性的值应该是能够执行的JavaScript代码

事件处理程序中的代码在执行时，有权访问到全局作用域中的任何代码

event是局部对象--在触发DOM上的某个事件时，会产生一个事件对象event，这个对象包含着所有与事件有关的信息

```html
<button onclick="alert(event.type)">点我</button>
```

**缺点：**

- 时差问题：用户可能会在HTML元素一出现在页面上就触发相应的事件，但是当时的事件处理程序还没有加载完成。解决方法：将事件封装在try-catch中
- 作用域问题：这样扩展事件处理程序的作用域链在不同浏览器中会导致不同结果，
- HTML和JavaScript代码紧密耦合： 如果要更换事件处理程序，就必须改动两个地方--HTML代码和JavaScript代码。

## DOM0级事件处理程序

**每个元素（包括window和document）都有自己的事件处理程序，这些属性通常全部小写，例如onclick。将这种属性的值设置为一个函数，就可以指定事件处理程序**

通过JavaScript指定事件处理程序的（在js中取得要操作对象的引用），将一个函数赋值给事件处理程序属性

```js
var btn = document.getElementById('myBtn')
btn.onclick = fucntion () {
    // do sth
    alert(this.id) // myBtn
}
```

在第四代Web浏览器中出现，沿用至今，原因一是简单，二是具有跨浏览器的优势

但是要注意，**在这些代码运行之前不会指定事件处理程序**，因此如果这段代码在页面中位于按钮后面，有可能在一段时间内点击没有反应

使用DOM0级方法指定的事件处理程序被认为是元素的方法，因此，这时候的事件处理程序是在元素的作用域中执行，即**程序中的this引用当前元素**

## DOM2级事件处理程序

“DOM2级事件”定义了两个方法

1. addEventListener：指定事件处理程序
2. removeEventListener：删除

所有DOM节点都包含这两个方法，接收三个参数：

- 要处理的事件名：如onclick
- 作为事件处理程序的函数
- 布尔值：true表示在捕获阶段调用，false表示在冒泡阶段调用

可以添加多个事件处理程序，按照添加的顺序触发

```js
var btn = document.getElementById('myBtn')
btn.addEventListener("click", fucntion () {
    // do sth
    alert(this.id) // myBtn
},false)
```

**通过addEventListener添加的事件处理程序只能使用removeEventListener来移除**

大多数情况下，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度兼容各种浏览器

## IE事件处理程序

类似于DOM

- attachEvent
- detachEvent

只接受两个参数

```js
var btn = document.getElementById('myBtn')
btn.attachEvent("onclick", fucntion () {
    // do sth
    alert(this === window) // true
})
```

区别：

- 作用域：this指向window
- 添加多个时按相反顺序触发

## 跨浏览器事件处理程序

```js
var EventUtil = {
    addHandler:function(element,type,handler){
        if(element.addEventListener){//检测是否存在DOM2
            element.addEventListener(type,handler,false)
        }else if(element.attachEvent){//存在ie
            element.attachEvent('on'+type,handler)
        }else{//DOM0
            element['on'+type]=handelr;
        }
    },
    removeHandler:function(element,type,handler){
        if(element.removeEventListener){
            element.removeEventListener(type,handler,false);
        }else if(element.detachEvent){
            element.detachEvent('on'+type,handler);
        }else{
            element['on'+type]=null;
        }
    }
}

var btn = document.getElementById('myBtn');
var handler = function(){
    console.log('hi')
}
EventUtil.addHandler(btn,'click',handler);

EventUtil.removeHandler(btn,'click',handler);
```

# 事件对象

在触发DOM的某个事件时，会产生一个事件对象event，这个对象包含着所有与事件相关的信息

在事件处理程序内部，对象this始终等于currentTarget的值，而target则只包含事件的实际目标

- currentTarget：事件处理程序当前正在处理事件的那个元素
- target：事件的目标
- detail：事件相关的细节信息
- bubbles：事件是否冒泡
- cancelable：是否可以取消事件的默认行为
- eventPhase：调用事件处理程序的阶段，1表示捕获阶段，2表示处于目标，3表示冒泡阶段
- type：被触发事件的类型
- view：与事件关联的抽象试图，等同于发生事件的window对象

# 事件类型

## UI事件

指的是不一定与用户操作有关的事件

### load

当页面完全加载后，包括所有图像、JavaScript文件、CSS文件等，就会触发window上的load事件

## 焦点事件

在页面获得或失去焦点时触发

- blur：元素失去焦点时触发
- focus：获得焦点时触发

## 鼠标与滚轮事件

- click：单击
- dblclick：双击
- mousedown：按下任意鼠标按钮
- mouseenter：在鼠标光标从元素外部首次移到元素范围之内触发
- mouseleave：鼠标光标离开
- mousemove：在元素内部移动时会重复触发
- mouseout：移入另一个元素
- mouseover：
- mouseup：释放按钮

## 键盘与文本事件

- keydown
- keyup
- keypress

# 内存与性能

## 事件委托

利用事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件

## 移除事件处理程序

每当将事件处理程序指定给元素时，运行中的浏览器代码与支持页面交互的JavaScript代码之间就会简建立一个连接，当连接越来越多，页面执行起来就会慢

除了采用事件委托限制连接数量，在不需要的适合移除事件处理程序也是解决这个问题的一种方案

# 模拟事件

可以使用JavaScript在任意时刻来触发特定的事件，此时的事件就如同浏览器创造的时间一样

## DOM

可以在document对象上使用createEvent方法创建event对象

这个方法接收一个参数，即表示要创建的事件类型的字符串

- UIEvents
- MouseEvents
- HTMLEvents
- MutationEvents

创建event对象后，需要使用与事件有关的信息对其进行初始化，每个类型的event对象都有一个初始化方法，传入适当的数据就可以初始化该event对象

最后一步是触发事件，调用dispatchEvent方法，传入event对象

## IE



