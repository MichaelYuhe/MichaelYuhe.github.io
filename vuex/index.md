# Vuex使用小结


## Vuex使用小结

### 写在前面

虽如官方文档所说：“如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex”。没错，我的应用就是够简单的那个:joy:

但随着深入学习，迟早也需要掌握Vuex的使用，何不现在就用上他呢？

### Vuex是什么

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式 + 库**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。（摘自Vue3官方文档）

该状态自管理应用包含三部分

- state，状态，驱动应用的数据源
- view，视图，以声明方式将state映射到视图
- actions，操作，响应在视图上的用户输入导致的状态变化

![image-20211001153324643](/images/vuex/image-20211001153324643.png)

Vuex可以帮助我们更好地管理共享状态

### 开始使用Vuex

#### 引入

```
npm i install vuex
```

在src文件夹下新建store目录，在其中新建index.js文件

``` js
import { createStore } from "vuex"

export default createStore({
    state: {},
    mutations: {},
    actions: {},
    modules: {}
})
```

#### 实例

往state中存入我们所需要共享的数据和操作

```js
state: {
	isLogin: false,
    configs: [
      {
        title: "theme",
        property: true,
        id: 1
      },
      {
        title: "enableDelete",
        property: true,
        id: 2
      }
    ]
},
mutations: {
    login(state) {
        state.isLogin = true
    }
}
```

存放好了数据，该怎么查看、使用和更改这些数据呢？

要调用这些数据，需要通过store.state来获取

```js
// 任意组件/视图下
console.log(this.$store.state.isLogin)
```

我们不能直接更改数据，需要通过提交mutation的方式来改变。例如，当我们点击了登录按钮

```js
methods: {
	login() {
		this.$store.commit('login')
	}
    // 当需要同时传递数据
    this.$store.commit('mutationName', data1, data2)
}
```

### 解决页面刷新数据丢失问题

store里的数据是保存在运行内存中的，每当页面刷新，就会重载Vue实例，store里的数据就会被重新赋值初始化。

要解决这个问题，就需要想办法把数据存储在本地（localStorage, sessionStorage, cookie三者之一中）。[三者区别](/#)

由于创建的是单页面应用，操作只是在一个页面跳转路由，因此选择sessionStorage.

具体操作：

1. 在页面初始化和每次更新时，读取sessionStorage的数据存储到store中，使用Vuex的replaceState方法来替换store的根状态
2. 在beforeunload方法中，将store.state存储到sessionStorage.

```js
// App.vue
created() {
    //在页面加载时读取sessionStorage里的状态信息
    if (sessionStorage.getItem("store")) {
      this.$store.replaceState(
        Object.assign(
          {},
          this.$store.state,
          JSON.parse(sessionStorage.getItem("store"))
        )
      );
    }
    //在页面刷新时将vuex里的信息保存到sessionStorage里
    window.addEventListener("beforeunload", () => {
      sessionStorage.setItem("store", JSON.stringify(this.$store.state));
    });
  },
```


