# React事件机制


# React事件机制

## DOM事件流

要学习React的事件机制前，需要先回顾一下DOM事件流。它包含三个流程：事件捕获，处于目标，事件冒泡。

![img](React%E4%BA%8B%E4%BB%B6%E6%9C%BA%E5%88%B6/898684-20200624143429777-917104228.png)

- 事件捕获阶段：事件对象通过目标节点的祖先Window传播到目标的父节点
- 处于目标阶段：事件对象到达了目标节点
- 事件冒泡阶段：事件对象以相反的顺序传播，到达Window结束

## React事件

React重新定义了自己的事件系统，其事件被称作**合成事件**

### 为什么要重新定义

- 抹平不同的浏览器之间的兼容性差异
- 方便自定义事件
- 提供一个抽象的跨平台事件机制
- 方便做更多优化。比如利用了事件委托机制，几乎所有事件的触发都代理到了document而非DOM节点本身，这样简化了DOM的事件处理逻辑，减少内存开销
- 干预事件分发

### 事件注册

React的事件注册主要有两个过程：在document上注册、存储事件回调

![img](React%E4%BA%8B%E4%BB%B6%E6%9C%BA%E5%88%B6/898684-20200624143507392-911347960.png)

#### 在document上注册

在组件挂载阶段，引擎根据组件内声明的事件（比如onClick，onChange等），在document上利用`addEventListener`注册事件，并指定统一的回调函数`dispatchEvent`。

换言之，不管在document上注册的是什么事件，它都具有统一的回调函数，所以对于同一种事件类型，最终只会保存一个有效实例，大大减少了内存开销

```react
function Click() {
  handleFatherClick=()=>{
		// ...
  }

  handleChildClick=()=>{
		// ...
  }

  return (
    <div className="father" onClick={this.handleFatherClick}>
		<div className="child" onClick={this.handleChildClick}> Child </div>
  	</div>
  )
}
```

上述代码中，注册的事件类型都是`onClick`，所以指定一个统一的回调函数，最终在document上只保留一个click事件

#### 存储事件回调

既然只保留了一个实例，那么它是怎么区分具体是哪个事件被触发的呢？

React 为了在触发事件时可以查找到对应的回调去执行，会把组件内的所有事件统一地存放到一个对象（`listenerBank`）中。

- 首先根据事件类型分类，一个类型对应一个对象
- 对象中采用键值对的方式存储事件，其中key是元素的唯一标识id，value对应的就是事件的回调函数

### 事件分发

- 触发事件，开始DOM事件流
- 事件冒泡到document的时候，触发了统一的事件分发函数`ReactEventListener.dispatchEvent`
- 根据原生事件对象找到触发事件的节点对应的`ReactDOMComponent`对象
- 事件合成
  - 根据当前事件类型生成对应的合成对象
  - 封装原生事件对象和冒泡机制
  - 查找当前元素及其所有父级
  - 在`listenerBank`中查找事件回调函数并合成到events，即合成事件中
- 批量执行events中的回调函数
- 如果没有阻止冒泡，就会继续冒泡到window

## 合成事件和原生事件的区别

- React 组件上声明的事件没有绑定在 React 组件对应的原生 DOM 节点上
- React 利用事件委托机制，将几乎所有事件的触发代理（`delegate`）在 document 节点上，事件对象(event)是合成对象(`SyntheticEvent`)，不是原生事件对象，但通过 `nativeEvent` 属性访问原生事件对象
- 由于 React 的事件委托机制，**React 组件对应的原生 DOM 节点上的事件触发时机总是在 React 组件上的事件之前**












