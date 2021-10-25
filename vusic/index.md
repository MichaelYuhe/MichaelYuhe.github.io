# Vusic


## 年轻人的第一个项目  Vusic

### 项目简介

Vusic是一个使用Vue3作为框架的在线音乐播放器，目前仅适配了移动端，桌面版的适配工作正在进行中。后端采用QQ音乐API来获取歌曲等数据。

以下是Vusic各界面截图及功能介绍。

播放界面

<img src="../../static/images/vusic/image-20211021224113172.png" alt="image-20211021224113172" style="zoom:67%;" />

【待补充】

### 开发中学到了什么

#### Composition API的使用

Vue3中新增了Composition API，代替之前的Options API，在逻辑、功能较多时，能使代码更简洁，有效降低耦合度，实现同一功能的代码分离到单独的文件，通过setup()调用以及返回到模板中，不需要再把所有data、所有methods等等都挤在同一个文件中。

#### 代码复用和模块化的思想

当有一段代码需要用到多次，不应该复制粘贴，而是需要学会将其封装起来，以方便复用。

要将大的项目模块化，更有助于我们的开发。在进行项目的开发之前，根据设计图和设计框架进行分析抽象，通过对结构和功能的划分来划分出具体的相应的模块。

学会封装一些基础的组件作为基本的模块，其他组件可能就只需要由已经封装好的基础组件拼装而成，不需要重新编写界面和逻辑。就如本项目中的音乐列表界面，可以从排行榜、歌手详情、推荐歌单等等进入，皆是他们的子路由，封装成基础组件，就减少了许多重复的工作。

而同一模块的代码和文件应该统一存放在一个文件夹中，方便代码的管理。

#### Sass的使用

Sass 是一款强化 CSS 的辅助工具，它在 CSS 语法的基础上增加了变量 (variables)、嵌套 (nested rules)、混合 (mixins)、导入 (inline imports) 等高级功能，这些拓展令 CSS 更加强大与优雅。使用 Sass 以及 Sass 的样式库有助于更好地组织管理样式文件，以及更高效地开发项目。

#### 后端接口的使用及数据的处理

后端数据由QQ音乐API获取

```js
// 对 axios get 请求的封装
function get(url, params) {
  return axios.get(url, {
    headers: {
      referer: 'https://y.qq.com/',
      origin: 'https://y.qq.com/'
    },
    params: Object.assign({}, commonParams, params)
  })
}

// 对 axios post 请求的封装
function post(url, params) {
  return axios.post(url, params, {
    headers: {
      referer: 'https://y.qq.com/',
      origin: 'https://y.qq.com/',
      'Content-Type': 'application/x-www-form-urlencoded'
    }
  })
}
```

#### 性能优化

##### keep-alive

使用Vue的keep-alive，缓存访问过的路由，有效地避免切换路由时重复多余的网络请求。

```vue
<router-view v-slot="{ Component }"> 
    <keep-alive>
      <component :is="Component"/>
    </keep-alive>
  </router-view>
```

##### 异步加载路由组件

用工厂模式导入路由组件，使路由组件达到按需加载的效果。

```js
const Singer = () => import('@/views/Singer'/* webpackChunkName: "singer" */)
```

#### 自定义指令

除了核心功能默认内置的指令 (`v-model` 和 `v-show`)，Vue 也允许注册自定义指令

本项目中，创建了三个自定义指令。`v-lazy`和`v-loading`和`v-no-result`

##### 注册指令：

使用`Vue.directive('command name')`来全局注册指令

##### 钩子函数

一个指令定义对象可以提供以下几个**钩子函数**

- `bind`：只调用一次，在指令第一次绑定到元素时调用
- `inserted`：被绑定元素插入父节点时调用
- `update`：所在组件的`VNode`更新时调用
- `componentUpdated`：指令所在组件的`VNode`和其子`VNode`全部更新后调用
- `unbind`：只调用一次，解绑时调用

##### 钩子函数参数

- `el`：指令所绑定的元素，可以用它来操作DOM
- `binding`：一个包含传递给钩子的参数的对象。有许多可用的参数，包括`name`，`value`，`oldValue`，`expression`，`arg`和`modifiers`。
- `vnode`：Vue编译生成的虚拟节点
- `oldVnode`：上一个虚拟节点

##### 自定义指令实例：

以本项目中自定义组件为例。由于几个自定义指令的逻辑大体相同，将其封装为`createLoadingLikeDirective`文件。

```js
// 创造自定义指令
export default function createLoadingLikeDirective(Comp) {
  return {
    mounted(el, binding) {
      const app = createApp(Comp)
      const instance = app.mount(document.createElement('div'))
      const name = Comp.name
      if (!el[name]) {
        el[name] = {}
      }
      el[name].instance = instance
      const title = binding.arg
      if (typeof title !== 'undefined') {
        instance.setTitle(title)
      }

      if (binding.value) {
        append(el)
      }
    },
    updated(el, binding) {
      const title = binding.arg
      const name = Comp.name
      if (typeof title !== 'undefined') {
        el[name].instance.setTitle(title)
      }
      if (binding.value !== binding.oldValue) {
        binding.value ? append(el) : remove(el)
      }
    }
  }

  function append(el) {
    const name = Comp.name
    const style = getComputedStyle(el)
    if (['absolute', 'fixed', 'relative'].indexOf(style.position) === -1) {
      appendClass(el, relativeCls)
    }
    el.appendChild(el[name].instance.$el)
  }

  function remove(el) {
    const name = Comp.name
    removeClass(el, relativeCls)
    el.removeChild(el[name].instance.$el)
  }
}
```

### 开发中的难点

【待补充】

### todos

- 支持创建歌单
- Material you设计
- 主题的切换
- 自动取色功能

### 下一阶段该做的事情

- 开发新的项目。项目选题：Material Vue（Vue项目样式改变的组件）/ 自定义生成头像的网站 / 记账工具；
- 学习计算机网络的知识
- 深入学习JavaScript

