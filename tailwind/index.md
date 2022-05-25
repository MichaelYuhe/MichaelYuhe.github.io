# Tailwind


# Tailwind

## 写在前面

做外包项目的时候，第一次使用了 Tailwind

第一感受就是：方便，再也不用写一大堆 CSS 和类名

然而面试的时候，面试官问我 Tailwind 的优点有哪些，我也只答得上方便了。面试官很不满意，就此一探究竟吧

## 什么是 Tailwind

Tailwind 是一个工具型的 CSS 框架，即 Utility CSS，它提供的 CSS Class 让我们可以设计、自定义任何组件

最大的特点就是上手难度低，开发体验好

## Tailwind 有哪些优势

### 简洁

对比行内样式，tailwind 最大的优势，自然就是简洁

### 样式规范

在使用 Tailwind 的时候，除了某些特殊情况需要自定义样式，大多数时候都是在使用官方制定好的样式规范，便自然而然地会写出有规范的样式

### 功能优先

没错，再也不用为了起一个好的类名想破头了。

除此之外，如果是多人维护的项目，如果没事先规范好的话，一堆类名实在是看不懂，维护和交接都有很大困难。

而使用 Tailwind 以后，所有的 class 都是功能性的。

### 方便的自定义

虽然说 Tailwind 给了一套完整且规范的 Utility Class 库，但有时我们不免需要自己制定一些特殊的样式，比如颜色，阴影等等。

此时就只需要在 `tailwind.config.js` 中自己增加几条规则即可。

### 轻量化

Tailwind 本身提内置了一个 purge 功能：只要设定完成，在项目 build 阶段就会自动将没有用到的 style 去除，最终 build 后的 css 文件一般都会很小，在 10kb 以内

### 好用的插件扩展

Tailwind 简化了样式的书写，但对于初学者往往会不清楚自己该怎么编写，就得来回在文档搜寻，降低效率。

Tailwind 就提供了 VScode 的相对应的插件，有自动提示功能，颜色还能在提示中预览。

### 深色模式

使用 Tailwind 能很方便地为自己的网站 / APP 添加深色模式

```html
<div class="bg-white dark:bg-gray-800">
  <h1 class="text-gray-900 dark:text-white">Dark mode is here!</h1>
  <p class="text-gray-600 dark:text-gray-300">
    Lorem ipsum...
  </p>
</div>
```

### 其他

除此之外，Tailwind 还有许多其他的优势，就不一一详细说明

- 响应式设计
- 对伪类的良好支持
- 提取组件
- 函数和指令
- 可以添加新的功能类

## Tailwind 有什么不足

### 不允许字符串拼接操作

这就给开发平添了一层复杂性，带有一定的风险，但可以靠引入其他库，比如 classNames 来解决

### 样式覆盖问题

```html
<div class="red blue"> </div>
```

上面的这个 div 是蓝色还是红色？并不能通过代码顺序确定，而是通过在内置的属性中的顺序确定










