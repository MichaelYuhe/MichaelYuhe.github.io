# call 和 apply 的模拟实现


# call 和 apply 的模拟实现

## call

### call 是什么

`call()`方法在使用一个指定的 this 值和若干个指定的参数值的前提下，调用某个函数或方法。

```javascript
const foo = {
	value: 1
}

function bar () {
    console.log(this.value)
}

// call 改变了 this 的指向，指向 foo
bar.call(foo)
```

### 如何实现 call

#### this 指向

换个方向思考，call 改变了函数 this 的指向，是不是相当于把这个函数变成那个要指向的对象的一个属性，就可以使得 this 自动指向该对象呢？

答案是肯定的。但需要注意一点，我们不能改变这个对象，所以执行完函数，就要把这个函数属性删除。

```javascript
Function.prototype.myCall = function (context) {
    // 把函数变成对象的一个属性
    context.fn = this
    // 执行函数
    context.fn()
    // 删除属性
    delete context.fn
}
```

#### 参数指定

call 不仅可以改变 this，还可以给定参数。

```javascript
var foo = {
    value: 1
};

function bar (name) {
    console.log(name)
    console.log(this.value);
}

bar.call(foo, 'MichaelYuhe');
// MichaelYuhe
// 1
```

但我们并不知道，使用 call 的时候会传递什么参数，传递的参数有几个，这时就需要用到 arguments 对象

比如上面的例子中

```javascript
arguments = {
    0: foo,
    1: 'MichaelYuhe',
    length: 2
}
```

我们就可以把参数取出，放到一个数组里面

```javascript
const args = [...arguments.slice(1)]
```

下一步，就是**把参数数组放到要执行的函数的参数中**，然后执行函数

```javascript
context.fn(...args)
```

所以代码如下

```javascript
Function.prototype.myCall = function (context) {
    context.fn = this
	const args = [...arguments.slice(1)]
    context.fn(...args)
    delete context.fn
}
```

#### 函数可以有返回值

```javascript

var obj = {
    value: 1
}

function bar (name) {
    return {
        value: this.value,
        name: name
    }
}

console.log(bar.call(obj, 'MichaelYuhe', 18));
// Object {
//    value: 1,
//    name: 'MichaelYuhe'
// }
```

解决方法很简单，添加一个 result 变量来存储返回值就可以了

#### this 可以是 null 

当 this 为 null，视为指向 window

```javascript
context = context || window
```

### 最终实现

```javascript
Function.prototype.myCall = function (context) {
    // context 为 null 则指向 window
	context = context || window
    // 添加属性
    context.fn = this
    // 提取参数
    const args = arguments.slice(1)
    // 给定参数执行，保存返回值
    const result = context.fn(...args)
    // 删除属性
    delete context.fn
    // 返回结果
    return result
}
```

## apply

### apply 是什么

`apply()` 方法调用一个具有给定 this 值的函数，以及作为一个数组或类似数组对象提供的参数。

### 如何实现 apply

call 和 apply 唯一的区别就在于传参方式

call 接收一个参数列表，apply 接收数组或者类数组对象

### 最终实现

```javascript
Function.prototype.myApply = function (context, arr) {
	context = context || window
    context.fn = this
    
    const result = arr ?
          context.fn() :
    		context.fn(...arr)
    
    delete context.fn
    return result
}
```

## 延伸阅读

### [什么是 arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)

### [删除函数属性](https://www.w3schools.com/howto/howto_js_remove_property_object.asp)


