# Node.js初体验


## Node.js初体验 --- nodemailer的使用

### 写在前面

需求：给自己的todo应用添加一个邮件提醒的功能，当创建了任务则会发送邮件至指定邮箱。

### 历程

作为一个Node.js小白，其他网络的知识也几乎没接触过，踩了许多的坑。

首先，经网上查找，找到了`nodemailer`这个库，兴致勃勃地下载，根据官方文档配置使用，在本地node环境中成功发送了测试邮件。

但当我把它放到我基于vue框架的todo应用中时，运行却报错了。`nodemailer`**仅可以在node环境中使用！**也就意味着项目需要前后端分离，并且需要前端与后端的交互。

没错，就是这个问题，一个简单的**前后端交互**困扰了我一整天。

最开始，自信满满的我，由于想把自己的项目都部署上线，本就有购买服务器的想法，直接去腾讯云购买了一个轻量应用服务器。将服务器配置了Node.js的应用镜像，将之前在本地node环境中运行的代码部署在服务器上直接运行，成功发送邮件至指定邮箱。

接着，就是分析逻辑：我要做的事情，无非就是向服务器**上传邮件具体内容**到服务器，同时**发起发送邮件的请求**，达到前端填写内容，按下发送按钮，服务端即可发送邮件的效果。

要能够向服务器发起请求，显然首先要能够访问到该服务器。在vue中引入了`axios`（没错这也是我第一次使用它），向服务器发起一个简单的`get`请求，结果却抛出了错误 `has been blocked by CORS policy`。

这是由于请求是**跨域**进行的。跨域的原因和解决方案后续将在另一篇文章[知识点总结]()中提到，此处仅关注这个需求。

`Vue`就自带了一个使用代理来解决跨域的方案。代理的作用：**监测本地的接口**，当接口为需要访问外网的接口时，**代理替你访问这个接口**并把返回值返回给当前网页

为方便测试，在本地起了一个node服务。

```js
// app.js
const express = require('express');
const app = express();
const email = require('./email');
app.get('/', (req, res) => {
    res.send('Hello')
})
app.use('/email', email);
// 监听端口3000
app.listen(3000, () => console.log('Service start at port 3000...'))
```

```js
// email.js
const express = require('express');
const email = express.Router(); // 创建express路由
const nodemailer = require('nodemailer'); // 导入nodemailer
const transporter = nodemailer.createTransport({
    // 以qq邮箱为例
    service: 'QQ',
    port: 465,
    secureConnection: true,
    auth: {
        user: '2312744987@qq.com',
        pass: '自行获取（SMTP授权码，而非邮箱密码）',
    }
});
email.get('/', (req, res) => {
    const { to, html, subject } = req.query; // 将请求解构
    const from = '"XYH"<2312744987@qq.com>';
    const mailOptions = {
        from,
        to,
        subject,
        html,
    };
    transporter.sendMail(mailOptions, (error, info) => {
        if (error) return console.log(error);
        console.log(info);
    });
});
module.exports = email;
```

在终端中输入命令 `node app.js`，即可在本地主机的3000端口启动服务。

那么在`Vue`中，如何配置代理呢？

来到我们项目中的`vue.config.js`文件，添加以下代码。

```js
devServer: {
        proxy: {
            'test': {
                target: 'http://localhost:4000',
                changeOrigin: true,
                logLevel: 'debug',
                pathRewrite: {'^/test' : '/'}
            }
            
        }
    },
```

那么当我们访问`/api/xxxx`接口时，`webpack`会识别到它是对外的接口，并访问target，将结果返回。接下来编写请求代码。

```js
axios({
        url: "test/email",
        method: "get",
        params: {
          to: "xiayuhang1106@gmail.com",
          subject: "A New Task",
          html: `<h2>${this.title}</h2>
          		<p>content<p/>`,
        },
      })
        .then((res) => console.log(res))
        .catch((err) => console.log(err));
```

上述代码发送了一个携带参数的axios请求，请求发送至配置好的代理端口的email中，请求发送电子邮件。

点击发送后，服务端打印出邮件信息，代表发送成功。

![image-20211027145751166](../../static/images/email/image-20211027145751166.png)

查看我们的邮箱，也可以看到该新到达的热乎的邮件。

下一步，就是在服务器端复现这个功能，省去每次都需要自己在本地启动服务的步骤。



### 结语

这是第一次真正意义上的接触Node.js，前前后后摸索了一天半，也学习到了很多知识：**跨域、Linux基本操作、服务器部署、axios**等。最后也成功实现了发送邮件的功能，虽然不太实用，后期需要想办法修改为定时发送的服务，到了设定时间前一天再发送邮件提醒。

不得不感慨真的是学无止境呀。还有许多东西需要自己去慢慢学习。

