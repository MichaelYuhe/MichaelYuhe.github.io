# Vue学习历程(一) Vue-todoist


### 前言

&ensp;&ensp;学习完基础的三剑客后，决定开始学习框架。无非就两种选择，**Vue**或是**React**，了解了一番二者的区别，再看着文档写了两个教程demo，二者具体深层的区别并没有太大了解也没有感受到，单纯觉得Vue更加适合我:joy:，于是就选择了学习Vue。至于Vue和React两个框架的区别，具体可以看看[这篇文章](https://zhuanlan.zhihu.com/p/100228073)

&ensp;&ensp;看完了文档，迫不及待想撸出自己第一个Vue应用，但仍不知道如何下手，对Vue应用的搭建的流程也并不了解，所以选择了观看一个很优秀的Youtuber -- [Traversy Media](https://www.youtube.com/c/TraversyMedia)的“Vue crash course”，看完视频后去复现他的代码，然后再自己修改样式、添加功能等等......

### 成品

![image-20210911093838040](/images/Vue-todoist/image-20210911093838040.png)

![image-20210911093755566](/images/Vue-todoist/image-20210911093755566.png)



- 点击Add Task按钮，弹出输入框，可以输入任务的具体内容和时间安排
- 待办的任务将会在下方显示，每个任务的右侧有两个功能按钮，分别是编辑和删除
- 数据储存于本地的json文件中，刷新浏览器或重新启动服务能照常读取数据

### todo

- 将json数据部署到云端，达到无需启动node服务即可访问的效果
- 调整布局，美化项目

