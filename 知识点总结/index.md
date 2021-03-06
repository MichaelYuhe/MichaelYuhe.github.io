# 知识点总结


# 知识点总结

## Javascript

### 讲一下闭包

#### 闭包的定义

**闭包是指那些能够访问自由变量的函数**。

那么什么是自由变量？**自由变量是指在函数中使用的，但既不是函数参数也不是函数的局部变量的变量**。

可以看出，**闭包 = 函数 + 函数能够访问的自由变量**

所以，**从技术的角度讲，所有的JavaScript函数都是闭包**。

从实际出发：

**即使创建它的执行上下文已经被销毁了，但它还是存在； 在其中引用了自由变量**

```js
var data = []
for(var i = 0; i < 3; i++) {
	data[i] = function() {
		console.log(i)
	}
}
data[0]() // 3
data[1]() // 3
data[2]() // 3
// 使用闭包
var data = []
for(var i = 0; i < 3; i++) {
	data[i] = (function(i) {
        return function() {
            console.log(i)
        }
    })
}
data[0]() // 0
data[1]() // 1
data[2]() // 2
// var改成let也可以
```



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

```js
function object(o) {
    function F() {}
    F.prototype = o;
    return new F();
}
function prototype(child, parent) {
    var prototype = object(parent.prototype);
    prototype.constructor = child;
    child.prototype = prototype;
}
// 使用的时候：
prototype(Child, Parent);
```

### 跨域原理和解决方案

#### 什么是跨域？

跨域是指**一个域下的文档或脚本试图去请求另一个域下的资源**，这里跨域是广义的。

而我们常说的跨域是狭义上的，是由浏览器同源策略限制的一类请求场景。浏览器的**同源策略**会导致跨域。

#### 同源策略

同源：**协议、域名和端口**都相同。

同源策略限制以下行为

- 无法读取cookie，localStorage和indexDB
- 无法获得DOM和js对象
- 不能发送AJAX请求

#### 跨域解决方案

1、 通过jsonp跨域
2、 document.domain + iframe跨域
3、 location.hash + iframe
4、 window.name + iframe跨域
5、 postMessage跨域
6、 **跨域资源共享（CORS）**
7、 nginx代理跨域
8、 **nodejs中间件代理跨域**
9、 WebSocket协议跨域

### JS是单线程还是多线程的？

#### JavaScript是单线程的：

JS的一大特点就是单线程。也就是说，同一时间只能做一件事。这是因为JavaScript是脚本语言，是为处理页面中用户的交互以及操作DOM产生的，所以若其是多线程，会带来很复杂的同步问题。所以为了避免复杂性，JavaScript一诞生起就是单线程，已经成为了这门语言的核心特征。

#### 既然JavaScript是单线程的，那么它怎么执行异步的代码？

单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。但JS引擎执行异步代码并不需要等待，是因为有**消息队列和事件循环**。

**消息队列**：消息队列是一个先进先出的队列，它里面存放着各种消息。
**事件循环**：事件循环是指主线程重复从消息队列中取消息、执行的过程。

### 事件循环 

 JavaScript是一门单线程的语言，但不意味着单线程就会阻塞，**实现单线程不阻塞的方法就是事件循环**。 

所有的任务都会分为**同步和异步**，同步直接放到主线程执行，异步任务进入任务队列，主线程的任务执行完了，就来消息队列取一个任务推到主线程执行。然后异步任务还可以细分成**宏任务和微任务**。每次执行一个宏任务，然后如果遇到微任务就把它放到微任务的任务队列。这个宏任务执行完了以后，就去看微任务队列，把里面所有微任务执行完。   

常见宏任务：setTimeout,setInterval,setImmediate;        

常见微任务：Promise.then() 

### JS中new具体做了些什么？

**new运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例**

new会进行哪些操作？

1. 创建一个简单的空的JavaScript对象，即{ }
2. 为步骤一创建出的对象添加属性`__proto__` ，将该属性链接至构造函数的原型对象
3. 将步骤一创建的对象作为this的上下文
4. 如果该函数没有返回对象，则返回this

### 手写new

```js
function objectFactory() {    
    // 新建一个对象    
    var obj = new Object()     
    Constructor = [].shift.call(arguments)    
    // 将obj的原型指向构造函数    
    obj.__proto__ = Constructor.prototype    
    // 使用 apply，改变构造函数 this 的指向到新建的对象res    
    var res = Constructor.apply(obj, arguments)    
    return typeof res === 'object' ? res : obj
}
```

### 立即执行函数

**IIFE(Immediately Invoked Function Expression)**,在定义后立即执行的JavaScript函数

```js
(function() {	statements;})();
```

第一部分是包围在()里的一个匿名函数，该函数拥有独立的块级作用域，避免了外界访问此IIFE的变量，也不会污染全局作用域。

第二部分再次使用()创建了一个立即执行函数表达式，JavaScript引擎到此将直接执行函数。

将 IIFE 分配给一个变量，不是存储 IIFE 本身，而是存储 IIFE 执行后返回的结果。

### 浮点数精度

#### 数字类型

ECMAScript中的Number类型使用**IEEE754标准**，全称 IEEE 二进制浮点数算术标准，来表示整数和浮点数值。

在 IEEE754 中，规定了四种表示浮点数值的方式：单精确度（32位）、双精确度（64位）、延伸单精确度、与延伸双精确度。像 ECMAScript 采用的就是双精确度，也就是说，会用 64 位字节来储存一个浮点数。

#### 浮点数转二进制

先看0.75转二进制

$0.75 = a * 2^{-1} + b * 2^{-2} + c * 2^{-3} ...$  两边同时乘2，

$1 + 0.5 = a * 2^{0} + b * 2^{-1} + ...$   得出a = 1，剩下的再乘2

$1 = b * 2^{0} + c * 2^{-1} + ...$  得出b = 1，其余为0

所以$0.75_{10} = 0.11_{2}$

但并非每一个小数都这么好计算，就比如$0.1 = 0.00011001100110011...$ 是无限循环的

#### 浮点数的存储

即使有些浮点数转成二进制是无限循环的，但我们仍需要存储它。在IEEE754中，一个浮点数表示方法为 $Value = sign * exponent * fraction$

比如说，0.1的二进制表示，就是$1 * 2^{-4} * 1.1001100110011...$ 当只做二进制科学计数法的表示，value可以更具体。$Value = (-1)^{sign} * (1 + fraction) * 2^{exponent}$ 

$(-1)^{sign}$表示符号位，sign为0为正数。再看$(1 + fraction)$，前面的1是共有的，不用存储，只需要存储1.xxxxx中的xxxxx。最后看$2^{exponent}$真正存储时，并不会直接存exponent，而是存$exponent + bias$。

所以要存储一个浮点数，只需存储sign、fraction、(exponent + bias)这三个值。其中sign用到一位，(exponent + bias)用到11位，fraction用到剩下的52位

![68747470733a2f2f67772e616c6963646e2e636f6d2f7466732f5442315666564579755432674b306a535a46765858586e465858612d3739302d3230352e6a7067](../../static/images/%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93/68747470733a2f2f67772e616c6963646e2e636f6d2f7466732f5442315666564579755432674b306a535a46765858586e465858612d3739302d3230352e6a7067.jpg)

所以当0.1这样的数被存储下来时，就已经发生了精度丢失。

#### 浮点数的运算

五个步骤：对阶、尾数运算、规格化、舍入处理、溢出判断

两次存储时的精度丢失加上一次运算时的精度丢失，导致$0.1 + 0.2 !== 0.3$

### 深拷贝和浅拷贝

#### 浅拷贝

浅拷贝，指的是创建新的数据，这个数据有着原始数据属性值的一份精确拷贝

如果属性是基本类型，拷贝的就是基本类型的值。如果属性是引用类型，拷贝的就是内存地址，即**浅拷贝是拷贝一层，深层次的引用类型则共享内存地址**

```js
// 浅拷贝的实现
// 1. 遍历复制
function shallowClone(obj) {
	let newObj = {}
	for(let prop in obj) {
		if(obj.hasOwnProperty(prop)) {
			newObj[prop] = obj[prop]
		}
	}
	return newObj
}
// 2. Object.assign()
```

#### 深拷贝

深拷贝**开辟一个新的栈，两个对象属完成相同，但是对应两个不同的地址，修改一个对象的属性，不会改变另一个对象的属性**

常见的深拷贝方式有：

- _.cloneDeep()
- jQuery.extend()
- JSON.stringify()
- 手写循环递归

```js
// 手写循环递归
function deepClone(obj, hash = new WeakMap()) {
  if (!obj) 
      return obj;
  if (obj instanceof Date) 
      return new Date(obj);
  if (obj instanceof RegExp) 
      return new RegExp(obj);
  if (typeof obj !== "object") 
      return obj;
  // 是对象的话就要进行深拷贝
  if (hash.get(obj)) 
      return hash.get(obj);
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash);
    }
  }
  return cloneObj;
}
```

### 讲一下Promise

#### Promise是什么

`Promise`是异步编程的一种解决方案，比传统的解决方案（回调函数）更加合理和更加强大。他解决异步操作的优点：

- 链式操作减低了编码难度
- 代码可读性明显增强

#### Promise的状态

- `pending`（进行中）
- `fulfilled`（已成功）
- `rejected`（已失败）
- Promise的状态不受外界的影响，只有异步操作的结果可以决定状态
- 状态改变是不可逆的



## CSS

### 双栏布局和三栏布局

#### 双栏布局

双栏布局非常常见，往往是以**一个定宽栏和一个自适应的栏**并排展示存在

实现思路

1. 使用`float`左浮左边栏；右边的自适应栏用`margin-left`撑开内容块；为父级添加`BFC`（如`overflow: hidden`），防止下方元素飞到上方内容。
2. 使用`flex`弹性布局。父元素添加`display: flex`左边设置定宽，右边设置`flex: 1`或者`width: 100%`

#### 三栏布局

- 两边使用`float`，中间使用`margin`
- 两边使用`absolute`，中间使用`margin`
- 两边使用`float`，并设置负的`margin`
- `display: table | flex | grid`



### BFC

Block Formatting Contexts (块级格式化上下文)。具有 BFC 特性的元素可以看作是**隔离了的独立容器**，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

### 水平居中

对于行内元素： `text-align: center;`

对于块级元素

```css
{
    margin: 0 auto;
}
{
    display: flex;
	justify-content: center;
}
{    
    position: absolute;    
    left: 50%;    
    transform: translate(-50%, 0);
}
```

### 垂直居中

对于行内元素

```css
.parent {	
    height: h;
}
.child {	
    line-height: h;
}
```

对于块级元素

**table**，元素高度可以动态改变

```css
.parent {	
    display: table;
}
.child {	
    display: table-cell;	
    vertical-align: middle;
}
```

**flex**，内容块宽高任意；可以用于更复杂高级的布局技术

```css
.parent {    
    display: flex;    
    align-items: center;
}
```

**绝对定位**

```css
.son {    
    position: absolute;    
    top: 50%;    
    transform: translate(0, -50%);
}
.son {    
    position: absolute;    
    top: 0;    
    bottom: 0;    
    margin: auto 0;
}
/* 适用于所有浏览器 */
.son {    
    position: absolute;    
    top: 50%;    
    height: 高度;    
    margin-top: -0.5高度;
}
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

单页面应用利用了JavaScript动态变换网页内容，**避免了页面重载**；**路由则提供了浏览器地址变化，网页内容也跟随变化**，两者结合起来则为我们提供了体验良好的单页面web应用。

#### hash模式

使用`window.location.hash`属性以及窗口的`onhashchange`事件，实现监听浏览器地址hash值变换，执行相应的js来切换网页

1. hash指的是地址中#号以及后面的字符，也称**散列值**。
2. **散列值并不随请求发送到服务器端**，所以改变hash不会重载页面
3. 当散列值改变，通过`window.location.hash`属性获取和设置hash值
4. `window.location.hash`的变化会直接反映到浏览器地址栏

#### history模式

`window.history`属性指向History对象，代表当前窗口的浏览历史。**History对象保存了当前窗口访问过的所有页面网址**，通过`history.length`可以知道当前窗口访问过几个网址。

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
| 与服务器端通信 | 每次都会被携带在HTTP请求头部中，若使用cookie保存过多数据会带来性能问题 | 仅在客户端保存，不与服务器端进行通信                        | 仅在客户端保存，不与服务器端进行通信                        |
| 易用性         | 需要程序员自己封装，源生的Cookie接口不友好                   | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 |

#### 应用场景

cookie：存储用户登录信息。针对登陆过的用户，服务器端会在他登陆时在cookie中插入一段加密过的唯一辨识单一用户的辨识码。cookie还需要指定作用域，不可以跨域调用。

localStorage可以用来夸页面传递参数，代替了cookie保存用户购物车信息的工作。sessionStorage用来保存一些临时的数据，防止用户刷新页面之后丢失了一些参数。

#### 安全性

并非所有数据都适合保存在上述三者中。只要打开控制台，就可以随意修改他们的值。所以重要的数据需要保存在服务器端而非客户端。



## 计算机网络

### HTTP常见状态码

`1xx`代表信息相应；`2xx`代表成功相应；`3xx`代表重定向；`4xx`代表客户端响应；`5xx`代表服务端响应。

- `100	Continue` ：表明到目前为止所有内容都是可行的，客户端可以继续请求
- `101    Switching Protocol`：指示服务器正在切换协议
- `200    OK`：请求成功，具体含义取决于请求的方法
- `204    Not Content`：服务器成功处理了请求，但不需要返回任何实体内容，并且希望返回更新了的元信息。响应可能通过实体头部的形式，返回新的或更新后的元信息。
- `205    Reset Content`：服务器成功处理了请求，且没有返回任何内容。但是与204响应不同，返回此状态码的响应要求请求者重置文档视图。如用户输入表单，提交后清空表单。
- `206    Partial Content`：服务器已经成功处理了部分 GET 请求。迅雷这类的 HTTP 下载工具都是使用此类响应实现断点续传或者将一个大文档分解为多个下载段同时下载。该请求必须包含 Range 头信息来指示客户端希望得到的内容范围，并且可能包含 If-Range 来作为请求条件。
- `301    Move Permanently`：被请求的资源已经永久移到了新位置。拥有编辑功能的客户端会自动把请求的地址修改为服务器返回的地址。
- `302    Found`：临时重定向，以后应该继续按原地址发送请求。
- `304    Not Modified`：客户端发送了一个带条件的 GET 请求且该请求已被允许，而文档的内容（自上次访问以来或者根据请求的条件）并没有改变，则服务器返回这个状态码。禁止包含消息体，因此始终以消息头后的第一个空行结尾。
- `400    Bad Request`：语义有误，服务器无法理解；请求参数有误。
- `401    Unauthorized`：当前请求需要用户验证。
- `403    Forbidden`：服务器已经理解请求，但是拒绝执行。
- `404    Not Found`：请求失败，请求需要的资源在服务器上未被发现。
- `500    Interval Server Error`：服务器遇到不知道该如何处理的情况。
- `503    Service Unavailable`：服务器没有准备好处理请求，常见原因是服务器因维护或重载而停机。

### TCP三次握手和四次挥手



### TCP和UDP的区别

#### UDP

用户数据包协议，一个简单的**面向数据的通信协议**，对于应用层传递下来的报文，它只会简单地加上首部，就交给网络层，不会做合并或者拆分的工作。特点如下

- 不提供复杂的控制机制，**利用IP提供面向无连接的通信服务**
- 传输过程中若出现了丢包，UDP也不会重发
- 包的到达顺序错乱了的话，UDP也无法纠正
- 无法进行流量控制等避免网络堵塞的行为
- 首部8个字节
- 发送端只负责将数据发送到网络，接收端从消息队列读取
- 支持一对一、一对多、多对一和多对多的交互通信

#### TCP

传输控制协议，是一种可靠的、**面向字节流的通信协议**，把应用层传递下来的数据看做是无结构的字节流来发送。，会根据当前网络的堵塞情况来确定每个报文段的大小。	

- 充分地实现了数据传输时的各种控制功能，可以进行重发控制、顺序控制
- 作为一种面向连接的服务，只有在确认通信对端的存在后才会发数据，控制通信流量的浪费
- 建立连接3次握手、断开连接四次挥手
- 根据TCP的机制，在IP这种无连接的网络上也能实现高可靠性的通信。
- 首部20字节
- 只能点对点全双工通信

### https和http的区别

### 七层网络模型

**应用层**

该层协议定义了应用进程之间的交互规则，通过不同的应用层协议为不同的网络应用提供服务；在应用层交互的数据单元称为报文。应用层协议：`HTTPS`、`DNS`、`SMTP`等

**表示层**

表示层的作用是使通信的应用程序能够解释交换数据的含义。该层提供的服务主要包括数据压缩，数据加密以及数据描述，使应用程序不必担心在各台计算机中表示和存储的内部格式差异

**会话层**

会话层就是负责建立、管理和终止表示层实体之间的通信会话。提供了数据交换的定界和同步功能，包括了建立检查点和恢复方案的方法

**传输层**

主要任务是为两台主机进程之间的通信提供服务，处理数据包错误、数据包次序，以及其他一些关键传输问题。主要的传输层协议是`TCP`和`UDP`

**网络层**

在网络层使用的协议是无连接的网际协议（Internet Protocol）和许多路由协议，因此我们通常把该层简单地称为 IP 层。

**链路层**

在物理层和网络层之间。两台主机之间的数据传输，总是在一段一段的链路上传送的，这就需要使用专门的链路层协议。将网络层交下来的 `IP`数据报组装成帧，在两个相邻节点间的链路上传送帧

**物理层**

和软件关系不大，确定与传输媒体的接口的一些特性（机械特性、电气特性、功能特性，过程特性）



## 性能

### 重排和重绘

#### 重排重绘是什么？

重排：也叫**回流**，当DOM的变化影响了**元素的几何信息**(DOM对象的位置和尺寸大小)，浏览器需要重新计算元素的几何属性，将其安放在界面中的正确位置，这个过程叫做重排。

**任何会改变元素几何信息(元素的位置和尺寸大小)的操作，都会触发重排**

- 添加或者删除可见的DOM元素
- 元素尺寸改变——边距、填充、边框、宽度和高度
- 内容变化，比如用户在input框中输入文字
- 浏览器窗口尺寸改变——resize事件发生时
- **计算 `offsetWidth` 和 `offsetHeight` 属性**
- 设置 style 属性的值

重绘：当一个元素的**外观发生改变**，但**没有改变布局**,重新把元素外观绘制出来的过程，叫做重绘。

**"重绘"不一定会出现"重排"，"重排"必然会出现"重绘"**

### 优化方法

- 尽可能减少重排的次数和范围：重排的性能花销跟渲染树有多少节点需要重新构建有关系，我们应尽量以局部布局的方式组织HTML
- **分离读写操作**，防止多次拿值触发渲染
- **样式集中改变**
- 缓存布局信息
- 离线改变DOM
- position可以多用absolute和fixed，这样不用考虑他对其他元素的影响

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

### 对keep-alive的理解

#### keep-alive是什么

`keep-alive`是`vue`中的**内置组件**，能在组件切换过程中将状态保留在内存中，防止重复渲染`DOM`，包裹动态组件时，会**缓存不活动的组件实例**，而不是销毁它们。它可以设置一些属性

- `include` 字符串或者正则表达式，只有名称匹配的组件才会被缓存
- `exclude` 和上面的相反，名称匹配的不被缓存
- `max` 最多可以缓存多少组件

#### keep-alive基本用法

```vue
<keep-alive>
  <component :is="view"></component>
</keep-alive>
```

设置了`keep-alive`缓存的组件，会多出两个生命周期钩子（`activated`与`deactivated`）

#### 使用场景

当我们在某些场景下不想让页面重新加载时我们可以使用`keepalive`

例如，当我们从首页切换到歌手页，再切换到收藏页面，再返回歌手页，我们不想每次切换页面都发送一次请求，就使用keep-alive将其缓存。

#### 原理分析

源码位置：`src/core/components/keep-alive.js`



### 通信方式

1. 通过 props 传递
2. 通过 $emit 触发自定义事件
3. 使用 ref
4. EventBus
5. $parent 或 root
6. attrs 与 listeners
7. Provide 与 Inject
8. Vuex

### 理解双向数据绑定

双向绑定由三个部分组成：数据层（Model）、视图层（View）、业务逻辑层（ViewModel）。其中最重要的是那个**ViewModel**，主要职责是**数据变化后更新视图，视图变化后更新数据**。包含两部分，监听器和解析器。监听器用来监听所有数据的属性，解析器对每个元素节点的指令进行扫描和解析，根据指令模板替换数据,以及绑定相应的更新函数。

简单来说，利用**Object.defineProperty()**，结合订阅-发布模式，对数据属性的getter和setter进行劫持。Vue3中抛弃了defineProperty()，改为Proxy

#### Proxy相比Object.defineProperty()的优势

`Object.defineProperty()`虽然可以做到监听对象属性，但是存在着一些缺点。

- **检测不到对象属性的添加和删除**
- **数组API无法监听到**，所以在Vue2中对数组API进行了重写
- 需要对每个属性进行遍历监听，如果是嵌套对象，需要**深层监听，造成性能问题**

而`Proxy`直接可以**劫持整个对象，并返回一个新对象**，我们只需要操作新对象就可以达到响应式目的。

`Proxy`的缺点：兼容性差，不兼容IE，而`Object.defineProperty()`可以兼容到IE9

#### **Vue里的双向绑定流程**

- 初始化，对data执行相应化处理，该过程发生在监听器
- 同时编译模板，找到里面动态绑定的数据，然后获取、初始化视图层，发生在解析器里
- 然后定义更新函数和一个Watcher，Watcher来观察数据是否变化了，如果变化就调用更新函数
- 然后因为某些data会出现多次，就要有多个watcher来观测他，所以对每一个data就准备一个管家来管理那些watcher
- 如果data变了，就先找他对应的管家，然后通知里面的每一个观察者watcher

### computed和watch

#### computed

computed设计的初衷是为了**使模板中的逻辑运算更简单**, 在模板中有很多复杂的数据计算的话, 可以把该计算逻辑放到computed中

computed是计算属性的，会根据所依赖的数据动态显示新的计算结果, 该计算结果会被缓存起来。

应用场景：

1. 一些重复使用的数据或复杂及费时的运算，可以放入computed中进行计算, 然后会在computed中缓存起来, 下次就可以直接获取。
2. 如果需要的数据依赖于其他的数据, 我们可以把该数据设计为computed中。

**computed和methods的区别**

1. computed 是基于响应性依赖来进行缓存的。只有在响应式依赖发生改变时它们才会重新求值。而methods不是响应式的，每次调用都会重新求值
2. omputed中的成员可以只定义一个函数作为只读属性, 也可以定义成 get / set变成可读写属性, 但是methods中的成员没有这样的。

#### watch

watch是一个**对data数据的监听回调, 当依赖的数据变化时, 会执行回调**。在回调中，会传入newVal和oldVal两个参数。
Vue实例会在实例化时调用$watch()，遍历watch对象的每一个属性。

应用场景：

当在data中的某个数据发生变化时, 我们需要做一些操作, 或者当需要在数据变化时执行异步或开销较大的操作时. 我们就可以使用watch来进行**监听**

#### watch和computed的异同

相同：都是观测页面数据变化的

不同：computed只有当依赖的数据变化时才会计算, 当数据没有变化时, 它会读取缓存数据。watch每次都需要执行函数。watch更适用于数据变化时的异步操作。

### Vue3新特性

### diff算法

#### 是什么

`diff` 算法是一种通过**同层的树节点**进行比较的高效算法，在 `vue` 中，作用于虚拟 `dom` 渲染成真实 `dom` 的新旧 `VNode` 节点比较。

它有两个特点

- 比较只会在相同层级进行，不会跨级比较
- 在具体某一层的diff比较中，循环从两边向中间比较

#### 原理分析

数据发生改变，`set`方法会调用`Dep.notify`通知所有订阅者`Watcher`，订阅者就会调用`patch`给真实的`DOM`打补丁，更新相应的视图

先看`patch`。接收四个参数，第一个是旧节点`oldVnode`，第二个是新节点`vnode`。

- 没有新节点，那么直接把旧的删光，触发旧节点的`destroy`钩子
- 没有旧节点，直接生成新的就可以了，`createElm`
- 通过`sameNode`判断两个节点一不一样，一样的话直接`patchVnode`，不一样的话创建新节点，删除旧节点



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


