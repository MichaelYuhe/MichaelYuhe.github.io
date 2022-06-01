# 

# 变量提升

## 什么是变量提升

**变量提升（Hoisting）被认为是，JavaScript 当中执行上下文，特别是创建和执行阶段的工作方式的一种认识。**

从概念的字面意义上说，“变量提升”意味着变量和函数的声明会在物理层面移动到代码的最前面，但这么说并不准确。实际上变量和函数声明在代码里的位置是不会动的，而是在编译阶段被放入内存中。

通俗地说，变量提升是指：JavaScript 代码执行过程中，引擎把变量和函数的声明部分提升到代码开头的行为

## 变量提升的体现

在 ES6 之前，我们只有烦人的 `var`

在`var`时代，任何变量都会被提升到作用域的顶端。

```javascript
console.log(num) // undefined
var num = 0
```

除此之外，函数也存在提升。

JavaScript 中，具名的函数有两种声明形式

```javascript
// 函数声明式
function foo () {}
// 变量声明式
var foo = function () {}
```

二者的区别在于，当使用变量声明式，它和普通的变量一样存在提升现象

而当使用函数声明式，它会连带声明内容一起提升

```js
foo()
var foo = function () {
	console.log(1)
}
// Uncaught TypeError: foo is not a function

bar()
function bar () {
	console.log(2)
}
```

## 变量提升带来的问题

### 变量覆盖

```js
var num = 1
function printNum () {
    console.log(num)
    var num = 2
}
printNum() // undefined
```

按照顺序，没有 JavaScript 经验的人以正常的逻辑分析，应该打印出的是 1

然而 `var num = 2`中的`var num`被提升，并且覆盖了之前的变量

### 变量不会被销毁

```js
function foo () {
	for (var i = 0; i < 5; i++) {
		
	}
	console.log(i) // 5
}
foo()
```



## 禁用变量提升

为了解决上述问题，ES6 就引入了 let 和 const 关键字，使得 JavaScript 也能够拥有块级作用域。
