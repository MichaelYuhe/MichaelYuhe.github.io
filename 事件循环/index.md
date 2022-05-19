# 事件循环


# 事件循环

### 什么是事件循环

众所周知，JavaScript是单线程的，一次只能运行一个任务。幸运的是，浏览器为我们提供了`Web API`，包含了`DOM API`、`setTimeout`、`HTTP`请求等等，可以帮助我们创建一些异步的、非阻塞的行为。

- 当我们调用一个函数，它会先**被添加到执行栈中**。当函数返回一个值的时候，就被移出执行栈。

  ![img](../../static/images/%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF/event-loop1.gif)

  如上图，respond函数返回了一个`setTimeout`函数，允许我们在不打破主线程的情况下延迟执行任务。我们传递给`setTimeout`的回调函数就被添加到了`Web API`中。

- `Web API`监听回调

  ![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/event-loop2.gif)

  我们之前设定了定时器，运行时间为1000ms。等待1000ms后，这个回调函数并不是被立即添加到执行找中，而是**被送到了队列中**

  ![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/event-loop3.gif)

  进入到队列以后，函数需要排队，等待轮到它执行时才能被执行。

- 那么队列里的任务什么时候执行呢？

  如果执行栈为空：也就是之前所有调用的函数都返回了，并且被移出了执行栈，**任务队列中的第一个任务，就会被添加到执行栈**。

  ![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/event-loop4.gif)

**事件循环机制，指的就是这个过程：监听任务队列和执行栈，若执行栈为空，则将任务队列中的第一个任务移入执行栈**

那么哪些任务会直接放到执行栈，哪些任务会被放到任务队列呢？

简单来说，可以把任务分成同步任务和异步任务。

![img](https://static.vue-js.com/61efbc20-7cb8-11eb-85f6-6fac77c0c9b3.png)

同步任务进入主线程，即主执行栈，异步任务进入任务队列，主线程内的任务执行完毕为空，会去任务队列读取对应的任务，推入主线程执行。上述过程的不断重复就事件循环

### 宏任务与微任务

然而事情并不是这么简单。让我们来看一个例子。

```javascript
console.log(1)

setTimeout(() => {
    console.log(2)
}, 0)

new Promise((resolve, reject) => {
    console.log('New Promise!')
    resolve()
}).then(() => {
    console.log('then')
})

console.log(3)
```

先按照同步和异步的分类来分析代码。

- `console.log(1)`，同步任务，主线程执行
- `setTimeout()`，异步任务，放入`Event table`，0毫秒（虽然并不是如此精确）后把`console.log(2)`这个回调推入任务队列
- `new Promise`，同步任务，新建一个Promise
- `.then()`，异步任务，放到`Event table`，回调推入任务队列
- `console.log(3)`，同步任务，主线程执行

那么按照分析，结果应该是

```
1
New Promise!
3
2
then
```

然而实际运行一下，会发现结果是

```
1
New Promise!
3
then
2
```

明明`setTimeout`的回调更早进入，为什么会晚执行？

原因就在于，异步任务还可以细分为宏任务和微任务。

#### 微任务

执行时机：主函数执行结束之后，当前宏任务执行结束之前

- `Promise.then`
- `MutationObserver`
- `Object.observe`
- `process.nextTick`

#### 宏任务

顾名思义，时间粒度大，实行的时间间隔并不能完全精确控制

- script
- `setTimeout`、`setInterval`
- `UI rendering`
- `postMessage`、`MessageChannel`
- `setImmediate`、`I/O`

![img](https://static.vue-js.com/6e80e5e0-7cb8-11eb-85f6-6fac77c0c9b3.png)

执行机制：

- 执行一个宏任务，执行过程中，如果遇到微任务，就把它放到微任务的任务队列
- 当前宏任务执行完成后，会查看微任务的任务队列，然后依次执行完

以这个执行机制来看上面的例子。

- `console.log(1)`，同步任务，主线程执行
- `setTimeout()`，宏任务
- `new Promise`，同步任务，新建一个Promise
- `.then()`，微任务，放入微任务队列
- `console.log(3)`，同步任务，主线程执行
- 本轮宏任务执行完毕，结束前去查看微任务队列，发现任务并执行
- 执行下一个宏任务，也就是`setTimeout()`
