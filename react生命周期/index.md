# React生命周期


# React生命周期

## 生命周期流程

React的生命周期可以分为三个阶段。

### 创建阶段

创建阶段主要有以下几个生命周期方法

- `constructor`
- `getDerivedStateFromProps`
- `render`
- `componentDidMount`

#### `constructor`

创建实例过程会自动调用的方法。在方法内部，通过`super`关键字获取来自父组件的`props`

在该方法中，通常进行的操作是

- 初始化`state`
- 在`this`上挂载方法

#### `getDerivedStateFromProps`

是一个静态的方法，不能访问到组件的实例

执行时机：组件的创建和更新阶段，不论是`props`还是`state`变化都会调用

在每次`render`之前调用，第一个参数是即将更新的`props`， 第二个参数为上一个状态的`state`

返回一个新对象作为新的`state`，若不需要更新，返回`null`

#### `render`

用于渲染`DOM`结构，是类组件**必须实现**的方法

可以访问`state`和`props`属性

不能在方法内部`setState`，否则会触发死循环导致内存崩溃

#### `componentDidMount`

组件挂载到真实的`DOM`节点后执行，顺序在`render`后

用于执行一些**数据获取、事件监听**的操作

### 更新阶段

该阶段的函数主要为如下方法：

- `getDerivedStateFromProps`
- `shouldComponentUpdate`
- `render`
- `getSnapshotBeforeUpdate`
- `componentDidUpdate`

#### `getDerivedStateFromProps`

同创建阶段

#### `render`

也与创建阶段介绍的一致

#### `shouldComponentUpdate`

告知组件本身，基于当前的`props`和`state`**是否需要重新渲染组件**

执行时机：`props`或`state`更新时都会调用，通过返回一个布尔值告知是否需要重新渲染

和`render`一样，不能调用`setState`

#### `getSnapshotBeforeUpdate`

该周期函数在`render`后执行，执行时`DOM`元素还没有被更新

该方法返回的一个`Snapshot`值，作为`componentDidUpdate`第三个参数传入

此方法的目的在于**获取组件更新前的一些信息**，比如组件的滚动位置之类的，在组件更新后可以根据这些信息恢复一些UI视觉上的状态

#### `componentDidUpdate`

执行时机：组件更新结束

该方法可以**根据前后的`props`和`state`变化来做出相应的操作**，如获取数据，修改`DOM`样式等等

### 卸载阶段

#### `componentWillUnmount`

此方法用于组件卸载前，清理一些注册的监听事件，或者取消订阅的网络请求等

**一旦一个组件实例被卸载，其不会被再次挂载，而只可能是被重新创建**

## 新旧生命周期对比

新版的生命周期减少了以下三种方法：

- componentWillMount
- componentWillReceiveProps
- componentWillUpdate

新增了两个方法：

- getDerivedStateFromProps
- getSnapshotBeforeUpdate


