# 

# React之`useCallback`

## `useCallback`是什么

先看一下官网的介绍：

把内联回调函数以及依赖项数组作为参数传入`useCallback`，它将会返回该**回调函数的memorized版本**。该回调函数仅在某个依赖项改变的时候才会更新。

`useCallback(fn, deps)` equals to `useMemo(() => fn, deps)`



## `useCallback`的应用

再回到官方文档。

当你把回调函数传递给经过优化的并使用引用相等性去避免非必要渲染的子组件时，它将非常有用。

什么是引用相等性？什么是非必要渲染？官方的文档讲的有些模糊和简单，让我们深入地分析一次，彻底弄懂`useCallback`




