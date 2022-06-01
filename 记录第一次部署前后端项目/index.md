# 

# 记录第一次部署前后端项目

## 写在前面

这学期上了一个人工智能实验课，最后的大作业是人脸识别，同时要求要有交互界面

前端人当然不能去用`qt`写啦，直接`React`一把梭，`python`那边用`Flask`作后端的框架

除了算法和训练，前后端的搭建没什么难度，很快半天就搭好了，本地测试通过

但要上交作业给老师，总不能让老师自己安装个`node`环境来跑吧:joy:

尝试用`vercel`部署了一下，部署完前端部分才发现`vercel`只能做`serverless`的服务，不适合本项目，想要部署就需要有自己的服务器。刚好最近发现，微软的Azure有提供教育版的100刀金额，比较便宜的服务器一个月10刀……于是卷王就这样诞生了……

## 配置服务器

之前买过一次腾讯云的服务器，这次就算是第二次配置服务器，比较轻车熟路

装好`node`、`python`的基础环境，再配置一下`git`，把代码拉到服务器，再安装一下相关的依赖。就完成了所有前置工作了

## 项目运行

### 后端

使用`pip`安装好依赖，尝试开启后端服务`python3 app.py`

```shell
WARNING: This is a development server. Do not use it in a production deployment.
Use a production WSGI server instead
```

看起来之前写的代码只适用于开发环境，不适合用在开发环境，引入个`waitress`库即可

```python
from waitress import serve

serve(app, host='0.0.0.0', port=8080)
```

然后使用`nohup`来持久运行这个脚本，后端服务就开启在了服务器的8080端口了

### 前端

进入前端目录，安装好相关依赖，然后进行打包`npm run build`

啪，又报错了

```shell
react-scripts: not found
```

好家伙，原来服务器上还要另外自己装`react-scripts`的吗

手动安装一下，
