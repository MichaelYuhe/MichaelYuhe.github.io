# Vue Router


## Vue学习历程（二） 路由的使用

### 写在前面

在着手进行一个大项目之前，想先优化、完善之前的todolist项目。例如：增加登录、注册功能；数据储存在云端；增加todo项目的分类与更清晰的状态表示；增加用户自定义设置，如皮肤，显示项等等……这些简单的功能放在单纯的一个页面中就显得拥挤，多页面又小题大做，也会影响使用体验，引入router就显得很有必要

### 什么是路由？

路由，可以根据不同的 url 地址展示不同的内容或页面，又有前端路由和后端路由之分，而后端路由的渲染存在性能问腿，所以在项目中前端路由发挥着重要的作用。

简单来说，前端路由就是在保证只有单页面，且交互时不刷新不跳转，为SPA(single page web application)中的每个视图展示形式匹配一个特殊的url。在刷新、前进后退等操作均通过这个特殊url来实现。

### Vue中如何使用路由？

路由一般有两种模式，分别是**hash模式**和**History模式**，此处将使用history模式(除非为了兼容IE8，均推荐使用History模式)

#### 前置工作

- 在项目所在目录打开终端，输入以下代码

  ```
  npm i install vue-router
  ```

- 在src文件夹下新建目录**route**，在该文件夹里添加一个**index.js**文件

  ```js
  // index.js
  import { createRouter, createWebHistory } from "vue-router" // 引入
  const history = createWebHistory()
  const router = createRouter({
      history,
      // 具体的路由项
      routes: [
          {
              // 以新增的about视图为例
              path: '/About',
              name: 'About',
              component: () => import('../views/About.vue') // 使用懒加载的方式
          }
      ]
  })
  
  export default router // 将router导出
  ```

- 修改**main.js**文件

  ```js
  // main.js
  import { createApp } from 'vue'
  import App from './App.vue'
  import router from './route' // 引入
  createApp(App)
      .use(router) // 注入
      .mount('#app')
  
  ```

- 新建**views**文件夹，内部存放需要通过路由显示的视图

- 



#### 




