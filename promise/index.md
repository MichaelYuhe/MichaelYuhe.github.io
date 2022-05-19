# Promise


# Promise

## Promise是什么

**Promise 对象用于表示一个异步操作的最终完成 (或失败)及其结果值。**

一个Promise对象代表一个**在这个promise被创建出来时不一定已知的值**

## Promise解决了什么问题

Promise解决了回调地狱。我们有时候会遇到一些情况：要处理一些**依赖其他任务的任务**

看一个简单的例子。

- 通过某地址获得一张图片
- 成功加载后，调用`resizeImage`函数对图像进行压缩
- 压缩后调用`applyFilter`函数给图像加一个滤镜
- 保存图像并给出成功的反馈

```javascript
getImage('./image.png', (image, err) => {
    if(err) throw new Error(err)
    resizeImage(image, (resizedImage, err) => {
        if(err) throw new Error(err)
        applyFilter(resizedImage, (filteredImage, err) => {
            if(err) throw new Error(err)
            saveImage(filteredImage, (res, err) => {
                if(err) throw new Error(err)
                console.log("Successfully saved!")
            })
        })
    })
})
```

这就是所谓的回调地狱：许多嵌套的回调函数，依赖于前面的回调函数，代码难以阅读。为了解决回调地狱这个问题，就引入了Promise。

## 解析Promise

不妨动手实操一下。让我们使用构造函数来创建一个Promise。

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/code2.gif)

我们就可以发现，Promise是一个包含状态和一个值的对象。

### PromiseStatus

有三种可能值。

- fullfilled：promise已被解析，一切正常！
- rejected：promise遇到了一些问题，被拒绝了。
- pending：还没被解析也没被拒绝，处于等待状态

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/code3.png)

如图，回调函数实际上会收到两个参数。

- resolve，是Promise解析时调用的函数
- reject，Promise拒绝（遇到错误时）调用的函数

那么就看看，调用这两个函数时会发生什么事情吧。

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/code4.gif)

可以看到，调用resolve，promise的状态就是fulfilled。调用reject，状态就是rejected。而promise的value，是函数的传参。

回到之前的例子。Promise可以很好地帮我们解决回调地狱。

```javascript
function getImage(file) {
    return new Promise((res, rej) => {
        try {
            const data = readFile(file)
            resolve(data)
        } catch(err) {
            reject(new Error(err))
        }
    })
}
```

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/code8.gif)

很好，我们成功获得了已解析的数据的值。

### 处理Promise的方法

- `.then()`：promise解析后调用
- `.catch()`：拒绝后调用
- `.finally()`：无论解析了还是拒绝了，都会调用
- `Promise.race()`：传入一个元素为Promise实例的数组，谁先解析了谁先调用`.then()`里的回调函数
- `Promise.all()`：传入一个元素为Promise实例的数组，全部解析了才调用`.then()`里的回调函数

```javascript
getImage(file)
	.then(image => console.log(image))
	.catch(error => console.log(error))
	.finally(() => console.log('All done.'))
```

仔细看上述代码，then和catch中的参数是哪来的呢？

- `.then()`方法通过`resolve`收到参数

  ![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/code10.gif)

- `.catch()`方法通过`reject`收到参数

  ![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/code11.gif)

利用then和catch方法最终改写一下例子

```javascript
getImage('./image.png')
	.then(image => resizeImage(image))
	.then(resizedImage => applyFilter(resizedImage))
	.then(filteredImage => saveImage(filteredImage))
	.then(res => console.log('Successfully saved'))
	.catch(err => throw new Error(err))
```

显然，这个结构来得清晰多了！

## Async / Await

好的，Promise还是很难:joy_cat:

还好，ES7中引入了一种添加异步行为的新方法，帮助我们更容易地使用Promise。

通过引入`async`和`await`帮助我们创建隐式返回promise的异步函数。

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/async1.png)

除了让我们不需要自己显式地写promise，异步函数的真正强大之处是通过await体现的。通过await，我们可以在等待的值返回已解析的promise时暂停异步函数。

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/async3.gif)

首先遇到同步任务，直接打印

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/async4.gif)

接着，遇到了一个异步函数，运行它的函数体。第一行是个同步任务，直接打印

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/async5.gif)

到了第二行，看到了await关键字。首先，等待的值被执行：被弹出到调用栈，最终返回了一个已解析的promise，promise被解析后，引擎才来处理await。遇到了await，async函数就被**挂起**，函数体暂停执行，剩下的异步函数就会被丢到微任务队列中去。

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/async6.gif)

现在异步函数被挂起了，引擎就**跳出异步函数继续往下，在函数被调用的执行上下文中继续执行代码**

![img](https://ths.js.org/2020/12/13/%E7%94%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E8%A7%A3%E9%87%8A%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E5%92%8CPromise/async7.gif)

最后，调用栈中已经没有要执行的任务了，就去检查任务队列，发现还有微任务在排队呢。于是依次执行即可。
