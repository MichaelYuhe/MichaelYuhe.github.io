# new操作符详解


# new操作符详解

## new操作符是什么

在 JavaScript 中，new 操作符用于**创建一个给定构造函数的实例对象**

- 创建出来的实例对象可以访问到构造函数中的属性
- 可以访问到构造函数原型链中的属性

## new的流程

首先分析 new 具体做了哪些工作

- 创建一个新的对象
- 将对象与构造函数通过原型链连接起来
- 将构造函数中的 this 绑定到新建的对象上
- 根据构造函数的返回类型来做判断
  - 不是对象则返回新建的那个对象
  - 是对象就返回 result

## 手写new操作符

```js
function myNew(Func, ...args) {
    // 创建新对象
    const obj = {}
    // 将对象与构造函数通过原型链连接
    obj.__proto__ = Func.prototype
    // 将构造函数中的绑定到新对象
    const result = Func.apply(obj, args)
    // 判断返回值
    return result instanceof Object ? result : obj
}
```


