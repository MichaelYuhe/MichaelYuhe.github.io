# 知识点总结


# 知识点总结



## Javascript

### 讲一下闭包

#### 闭包的定义

**闭包是指那些能够访问自由变量的函数**。那么什么是自由变量？**自由变量是指在函数中使用的，但既不是函数参数也不是函数的局部变量的变量**。那么可以看出，**闭包 = 函数 + 函数能够访问的自由变量**

所以，**从技术的角度讲，所有的JavaScript函数都是闭包**。

### 原型和原型链



### 讲一下继承

#### 1.原型链继承

```js
function Parent() {
    this.name = 'Michael'
}
Parent.prototype.getName = function() {
    console.log(this.name)
}
function Child() {
    
}
Child.prototype = new Parent()
var child1 = new Child()
child1.getName() // 'Michael'
```

缺陷：

- 引用类型的属性被所有实例共享

  ```js
  function Parent() {
  	this.names = ['Michael', 'Milk+']
  }
  function Child() {
  	
  }
  Child.prototype = new Parent()
  var child1 = new Child()
  child1.names.push('Lily')
  console.log(child1.names) // ['Michael', 'Milk+', 'Lily']
  var child2 = new Child()
  console.log(child2.names) // ['Michael', 'Milk+', 'Lily']
  ```

- 在创建Child实例时不能向Parent传参

#### 2.借用构造函数（经典继承）

```js
function Parent() {
	this.names = ['Michael', 'Milk+']
}
function Child() {
	Parent.call(this)
}
var child1 = new Child()
child1.names.push('Lily')
console.log(child1.names) // ['Michael', 'Milk+', 'Lily']
var child2 = new Child()
console.log(child2.names) // ['Michael', 'Milk+']
```

优点：

- 避免了引用类型的属性被所有实例共享

- 可以在Child中向Parent传参

  ```js
  function Parent (name) {
      this.name = name
  }
  function Child (name) {
      Parent.call(this, name)
  }
  var child1 = new Child('kevin')
  console.log(child1.name);// kevin
  var child2 = new Child('daisy')
  console.log(child2.name) // daisy
  ```

缺点：

- 方法都在构造函数中定义，每次创建实例都会创建一遍方法

#### 3.组合继承

```js
function Parent() {
    this.name = name
    this.colors = ['black', 'pink']
}
Parent.prototype.getName = function() {
    console.log(this.name)
}
function Child(name, age) {
    Parent.call(this, name)
    this.age = age
}
Child.prototype = new Parent()
Child.prototype.constructor = Child
var child1 = new Child('Lily', 18)
child1.colors.push('blue')
console.log(child1.name) // Lily
console.log(child1.age) // 18
console.log(child1.colors) // ['black', 'pink', 'blue']
var child2 = new Child('Mike', 22)
console.log(child2.name) // Mike
console.log(child2.age) // 22
console.log(child2.colors) // ['black', 'pink']
```

优点：

- 融合了原型链继承和经典继承的优点，是JavaScript中最常用的继承方式

缺点：

- 调用了两次父构造函数

#### 4.原型式继承

```js
function createObj(o) {
	function F() {}
	F.prototype = o
	return new F()
}
```

缺点：

- 和原型链继承一样，包含引用类型的属性值始终都会共享相应的值

  ```js
  var person = {
  	name: 'Mike',
  	friends: ['Lily', 'Daisy']
  }
  var person1 = createObj(person)
  var person2 = createObj(person)
  person1.name = 'person1'
  console.log(person2.name) // Mike
  person1.friends.push('Cole')
  console.log(person2.friends) // ['Lily', 'Daisy', 'Cole']
  ```

  此处修改person1.name的值，可以看到person2.name并未改变，并非因为二者有独立的name值，而是因为是给person1添加了name值，而非修改原型上的name值

#### 5.寄生式继承

创建一个仅用于封装继承过程的函数，该函数在内部以某种形式来做增强对象，最后返回对象

```js
function createObj(o) {
	var clone = Object.create(o)
	clone.sayName = function() {
		console.log('hi')
	}
	return clone
}
```

缺点：

- 和经典继承一样，每次创建对象都会创建一次方法

#### 6.寄生组合式继承

**引用类型最理想的继承范式**



### JS是单线程还是多线程的？

#### JavaScript是单线程的：

JS的一大特点就是单线程。也就是说，同一时间只能做一件事。这是因为JavaScript是脚本语言，是为处理页面中用户的交互以及操作DOM产生的，所以若其是多线程，会带来很复杂的同步问题。所以为了避免复杂性，JavaScript一诞生起就是单线程，已经成为了这门语言的核心特征。

#### 既然JavaScript是单线程的，那么它怎么执行异步的代码？

单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。但JS引擎执行异步代码并不需要等待，是因为有**消息队列和事件循环**。

**消息队列**：消息队列是一个先进先出的队列，它里面存放着各种消息。
**事件循环**：事件循环是指主线程重复从消息队列中取消息、执行的过程。

### JS中new具体做了些什么？

**new运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例**

new会进行哪些操作？

1. 创建一个简单的空的JavaScript对象，即{ }
2. 为步骤一创建出的对象添加属性`__proto__` ，将该属性链接至构造函数的原型对象
3. 将步骤一创建的对象作为this的上下文
4. 如果该函数没有返回对象，则返回this

### 手写new

```js
function objectFactory() {    // 新建一个对象    var obj = new Object()    //    Constructor = [].shift.call(arguments)    // 将obj的原型指向构造函数    obj.__proto__ = Constructor.prototype    // 使用 apply，改变构造函数 this 的指向到新建的对象res    var res = Constructor.apply(obj, arguments)    return typeof res === 'object' ? res : obj}
```



### 变量提升

为什么可以先调用再声明？这是因为JavaScript的工作方式是执行上下文，在执行任何代码段之前，将函数声明放入内存。

JavaScript只会提升声明，而不会提升其初始化。



### 立即执行函数

**IIFE(Immediately Invoked Function Expression)**,在定义后立即执行的JavaScript函数

```js
(function() {	statements;})();
```

第一部分是包围在()里的一个匿名函数，该函数拥有独立的块级作用域，避免了外界访问此IIFE的变量，也不会污染全局作用域。

第二部分再次使用()创建了一个立即执行函数表达式，JavaScript引擎到此将直接执行函数。

将 IIFE 分配给一个变量，不是存储 IIFE 本身，而是存储 IIFE 执行后返回的结果。



## CSS

### BFC

Block Formatting Contexts (块级格式化上下文)。具有 BFC 特性的元素可以看作是**隔离了的独立容器**，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

### 水平居中

对于行内元素： `text-align: center;`

对于块级元素

```css
{	margin: 0 auto;}{    display: flex;    justify-content: center;}{    position: absolute;    left: 50%;    transform: translate(-50%, 0);}
```

### 垂直居中

对于行内元素

```css
.parent {	height: h;}.child {	line-height: h;}
```

对于块级元素

**table**，元素高度可以动态改变

```css
.parent {	display: table;}.child {	display: table-cell;	vertical-align: middle;}
```

**flex**，内容块宽高任意；可以用于更复杂高级的布局技术

```css
.parent {    display: flex;    align-items: center;}
```

**绝对定位**

```css
.son {    position: absolute;    top: 50%;    transform: translate(0, -50%);}.son {    position: absolute;    top: 0;    bottom: 0;    margin: auto 0;}/* 适用于所有浏览器 */.son {    position: absolute;    top: 50%;    height: 高度;    margin-top: -0.5高度;}
```













## HTML & 浏览器

### 讲一下DOM树

#### 什么是DOM？

从网络传给渲染引擎的 HTML 文件字节流是无法直接被渲染引擎理解的，所以要将其转化为渲染引擎能够理解的内部结构，这个结构就是 DOM。DOM 提供了对 HTML 文档结构化的表述。

简而言之，DOM 是表述 HTML 的内部数据结构，它会将 Web 页面和 JavaScript 脚本连接起来，并过滤一些不安全的内容

#### DOM的作用？

1. 页面的视角，生成页面的基本结构
2. Javascript脚本视角，提供脚本操作的**接口**
3. 安全视角，是一道安全防线，一些不安全的内容在DOM解析阶段就被拒绝

#### DOM怎么生成的？

渲染引擎内部，有一个叫HTML解析器的模块，负责将HTML字节流转换成DOM，网络进程加载了多少数据，解析器便解析多少数据

第一阶段，通过**分词器将字节流转换成Token**，分为Tag Token和文本Token

第二阶段（**将Token解析为DOM节点**）和第三阶段（**将DOM节点添加到DOM树**）是同时进行的

HTML解析器维护了一个Token栈结构，根据转换的Token来维护这个栈，进行出栈入栈的操作

#### DOM解析中的影响因素

遇到JavaScript脚本时，需要暂停整个DOM的解析，先执行JavaScript代码。原因：Javascript脚本可能更改原有的节点

可以通过**预解析**操作：当渲染引擎收到字节流之后，会开启一个预解析线程，用来分析 HTML 文件中包含的 JavaScript、CSS 等相关文件，解析到相关文件之后，预解析线程会提前下载这些文件  来优化

### 路由

#### 为什么要使用路由

单页面应用利用了JavaScript动态变换网页内容，避免了页面重载；路由则提供了浏览器地址变化，网页内容也跟随变化，两者结合起来则为我们提供了体验良好的单页面web应用。

#### hash模式

使用`window.location.hash`属性以及窗口的`onhashchange`事件，实现监听浏览器地址hash值变换，执行相应的js来切换网页

1. hash指的是地址中#号以及后面的字符，也称**散列值**。
2. 散列值并不随请求发送到服务器端，所以改变hash不会重载页面
3. 当散列值改变，通过`window.location.hash`属性获取和设置hash值
4. `window.location.hash`的变化会直接反映到浏览器地址栏

#### history模式

`window.history`属性指向History对象，代表当前窗口的浏览历史。History对象保存了当前窗口访问过的所有页面网址，通过`history.length`可以知道当前窗口访问过几个网址。

方法：`history.back(); history.forward(); history.go(); history.pushState(); history.replaceState()`

### 跨页面通信

#### 同源页面

- 广播模式：`Broadcast Channe / Service Worker / LocalStorage + StorageEvent`

- 共享存储模式：`Shared Worker / IndexedDB / cookie`

- 口口相传模式：`window.open + window.opener`

- 基于服务端：`Websocket / Comet / SSE` 

#### 非同源页面

嵌入同源 `iframe` 作为“桥”，将非同源页面通信转换为同源页面通信。

### 缓存机制

浏览器的缓存机制也就是我们说的**HTTP缓存机制**，其机制是根据HTTP报文的缓存标识进行的

### 本地存储

本地存储共有三种方式，`coookie`，`localStorage`和`sessionStorage`

#### 三者的异同

| 特性           | cookie                                                       | localStorage                                                | sessionStorage                                              |
| -------------- | ------------------------------------------------------------ | ----------------------------------------------------------- | ----------------------------------------------------------- |
| 生命周期       | 一般由服务器端生成，会设置失效时间。若没有设置，则默认关闭浏览器时清除 | 除非手动清除，否则永久保存                                  | 仅在当前会话下有效，关闭页面或浏览器后被清除                |
| 存放数据大小   | 4K左右                                                       | 5M                                                          | 5M                                                          |
| 与服务器端通信 | 每次都会被携带在HTTP请求头部中，若使用cookie保存过多数据会带来性能问题 | 尽在客户端保存，不与服务器端进行通信                        | 尽在客户端保存，不与服务器端进行通信                        |
| 易用性         | 需要程序员自己封装，源生的Cookie接口不友好                   | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 |

#### 应用场景

cookie：存储用户登录信息。针对登陆过的用户，服务器端会在他登陆时在cookie中插入一段加密过的唯一辨识单一用户的辨识码。cookie还需要指定作用域，不可以跨域调用。

localStorage可以用来夸页面传递参数，代替了cookie保存用户购物车信息的工作。sessionStorage用来保存一些临时的数据，防止用户刷新页面之后丢失了一些参数。

#### 安全性

并非所有数据都适合保存在上述三者中。只要打开控制台，就可以随意修改他们的值。所以重要的数据需要保存在服务器端而非客户端。



## 计算机网络



## webpack





## 性能

### 重排和重绘

#### 重排重绘是什么？

重排：也叫**回流**，当DOM的变化影响了**元素的几何信息**(DOM对象的位置和尺寸大小)，浏览器需要重新计算元素的几何属性，将其安放在界面中的正确位置，这个过程叫做重排。

**任何会改变元素几何信息(元素的位置和尺寸大小)的操作，都会触发重排**

- 添加或者删除可见的DOM元素
- 元素尺寸改变——边距、填充、边框、宽度和高度
- 内容变化，比如用户在input框中输入文字
- 浏览器窗口尺寸改变——resize事件发生时
- 计算 `offsetWidth` 和 `offsetHeight` 属性
- 设置 style 属性的值

重绘：当一个元素的**外观发生改变**，但**没有改变布局**,重新把元素外观绘制出来的过程，叫做重绘。

**"重绘"不一定会出现"重排"，"重排"必然会出现"重绘"**

### 优化方法

- 尽可能减少重排的次数和范围：重排的性能花销跟渲染树有多少节点需要重新构建有关系，我们应尽量以局部布局的方式组织HTML
- 分离读写操作，防止多次拿值触发渲染
- 样式集中改变
- 缓存布局信息
- 离线改变DOM
- position多用absolute和fixed，这样不用考虑他对其他元素的影响

### 白屏

#### 概念

白屏时间：即用户点击一个链接或打开浏览器输入URL地址后，**从屏幕空白到显示第一个画面的时间**。

#### 白屏期间发生了什么？

1. DNS Lookup

   浏览器会先对页面进行**域名解析**，获取到服务器的IP地址后，进而和服务器进行通信。

2. 建立TCP请求连接

3. 服务端处理响应

   在TCP连接建立后，Web服务器接受请求，开始进行处理，同时浏览器端开始等待服务器的处理响应。

4. 客户端下载、解析、渲染显示页面

#### 白屏优化

针对白屏期间的环节进行逐一优化

1. DNS缓存优化、DNS预加载、稳定可靠的DNS服务器
2. TCP网络链路优化
3. 服务端处理优化
4. 尽可能精简HTML、CSS代码和结构。合理放置JS代码



## Vue

### 数据绑定原理



## 操作系统

### **进程和线程的区别：**

**进程**，是程序的依次执行过程，是程序在执行过程中分配和管理资源的基本单位，每个进程都有自己的地址空间，至少有五种状态：初始态，执行态，等待状态，就绪状态，终止状态。

**线程**，是CPU调度和分派的基本单位，可以和同一进程下的其他线程共享全部资源

**二者联系**：线程是进程中的一部分，一个进程可以有多个线程，但一个线程只能存在于一个进程中。

**根本区别**：进程是操作系统资源分配的基本单位，而线程是任务调度和执行的基本单位。
**开销方面**：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。
**所处环境**：在操作系统中能同时运行多个进程（程序）；而在同一个进程（程序）中有多个线程同时执行（通过CPU调度，在每个时间片中只有一个线程执行）
**内存分配方面**：系统在运行的时候会为每个进程分配不同的内存空间；而对线程而言，除了CPU外，系统不会为线程分配内存（线程所使用的资源来自其所属进程的资源），线程组之间只能共享资源。
**包含关系**：没有线程的进程可以看做是单线程的，如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。


