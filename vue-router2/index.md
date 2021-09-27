# Vue Router2


## Vue 3学习之路由的进阶

### 写在前面

之前学习使用了vue的路由，实现了简单的单页面内部pages跳转，继续跟随官方视频学习，进阶vue3中路由的使用

### 切换效果

vue-router提供了一套完备方便的，用于给各路由页面切换增加效果的方法。

在vue2中，我们可以使用如下方法给页面切换添加slider效果

```html
<transition name="slide" mode="out-in">
    <router-view :key="$route.path"/>
</transition>
```

```css
.slide-enter-active,
.slide-leave-active {
  transition: opacity .6s, transform .6s;
}

.slide-enter,
.slide-leave-to {
  opacity: 0;
  transform: translateX(-30%);
}
```

但在vue3中，继续这种写法能正常工作，但会有warning。

![image-20210927191423295](/images/vue-router2/image-20210927191423295.png)

根据warning提示对其进行修改即可

```
```

### 路由回溯与追踪

想要添加一个回到上一页的功能却苦于状态管理？Vue已经帮我们准备好了方法。很简单，仅需要一行代码。给一个按钮绑定goBack事件，点击触发，以下是goBack函数内的所有内容。this.$router.go()可以接受正数和负数作为参数，来进行路由间的跳转。

```js
return this.$router.go(-1) 
```

### 嵌套路由

一些应用程序的 UI 由多层嵌套的组件组成。在这种情况下，URL 的片段通常对应于特定的嵌套组件结构。通过 Vue Router，就可以使用嵌套路由配置来表达这种关系。

想要将组件渲染到嵌套的router-view当中，就需要在路由中配置chidren

```js
{	path: "/details/:slug",
    name: "DestinationDetails",
    component: () => import(/* webpackChunkName: "DestinationDetails" */"../views/DestinationDetails.vue"),
    props: true,
    children: [
      {
        path: ":experienceSlug",
        name: "ExperienceDetails",
        component: () => import(/* webpackChunkName: "ExperienceDetails" */"../views/ExperienceDetails.vue"),
        props: true
      }
    ]}
```

如上所示，ExperienceDetails组件就将被渲染到嵌套在DestinationDetails组件的router-view中，而不会跳转到新的页面。

`children` 配置只是另一个路由数组，就像 `routes` 本身一样。因此，可以根据自己的需要，不断地嵌套视图。

### 解耦：路由组件传参

在组件中过多使用`$route`会使组件与路由紧密耦合，限制了其灵活性，因为这样只能用于特定的URL，我们可以通过配置props来解决这个问题。以官方文档为例，我们可以将下面的代码

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const routes = [{ path: '/user/:id', component: User }]
```

替换成

```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const routes = [{ path: '/user/:id', component: User, props: true }]
```

这就允许我们在任何地方使用该组件，使其更易于复用与测试。

### 导航守卫

**正如其名，vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航**

### 

### 

