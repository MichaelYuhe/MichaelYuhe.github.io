# 一键回到顶部


## 如何给自己的博客添加一键回到顶部的功能？

&ensp;&ensp;今天用Hugo搭好了博客，美化、自定义什么的先放在一边，单纯用框架改改参数什么的，总感觉技术含量不高嘛（高了我也做不出来），就想着先添加个小功能，提供一个一键回到顶部的功能。让我们开始吧。

&ensp;&ensp;本文以**Hugo**框架，**LoveIt**主题为例。

- ### 添加样式

  在CSS中，定义**返回顶部按钮**的样式。打开Hugo主题目录下的 static/asserts/_custom.scss，在其中加入样式代码。

  ```css
  .backtop {
      color: #b3b1b1;
      position: fixed;
      right: 16px;
      bottom: 20px;
      width: 33px;
      height: 33px;
      z-index: 10000;
  }
  ```

- ### 导入按钮图标

  准备好按钮的图标，前往[Font Awesome](https://fontawesome.com/v5.15/icons)找到心仪的图标，下载其SVG文件，打开后全部复制

  在Hugo主题目录下layouts/partials新建back2top.html文件，将拷贝的代码放入

  ```html
  <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="chevron-circle-up" class="svg-inline--fa fa-chevron-circle-up fa-w-16" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
      <path fill="currentColor" d="M8 256C8 119 119 8 256 8s248 111 248 248-111 248-248 248S8 393 8 256zm231-113.9L103.5 277.6c-9.4 9.4-9.4 24.6 0 33.9l17 17c9.4 9.4 24.6 9.4 33.9 0L256 226.9l101.6 101.6c9.4 9.4 24.6 9.4 33.9 0l17-17c9.4-9.4 9.4-24.6 0-33.9L273 142.1c-9.4-9.4-24.6-9.4-34 0z">
      </path>
  </svg>
  ```

- ### **添加锚点**

  在当前目录下找到header.html文件并打开，在任意行加入以下代码

  ```html
  <span id="top"></span>
  ```

  来到layouts/_default文件夹，打开single.html，添加以下代码(与content同级)

  ```html
  {{- define "main" -}}
  <a href="#top" class="backtop">{{ partial "back2top.html" . }}</a>
  ```

- ### 大功告成！


