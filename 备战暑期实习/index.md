# 备战暑期实习


# 备战暑期实习

## 计算机网络

### TCP三次握手四次挥手

#### 三次握手

三次握手，指的就是建立一个TCP链接时，需要客户端和服务器共发送三个包，主要作用是确认双方的收发能力正常、指定自己的初始化序列号，为后面的可靠性传输做准备

- 第一次握手，客户端给服务端发一个SYN报文，其中包含客户端的初始化序列号ISN，客户端变为SYN_SENT状态。作用：**服务端得出结论，客户端的发送能力和服务端的接收能力正常**
- 第二次握手，服务端接收到客户端发来的SYN报文后，以自己的SYN报文作为应答，并将客户端的ISN+1作为ACK的值。服务端变为SYN_RCVD状态。作用：**客户端得出结论，客户端的收发能力、服务端的收发能力正常**
- 第三次握手，客户端收到SYN报文后，发送一个ACK报文，值是服务端的ISN+1。客户端变为ESTABLISHED状态；服务端接收到ACK报文后，也变成ESTABLISHED状态，双方成功建立了链接。作用：**服务端知道客户端的接收能力和服务端的发送能力正常**

如果是两次握手，会发生什么？

- 服务端不知道客户端的接收能力和自己的发送能力是否正常
- 客户端可能会因为网络阻塞等原因，发送多个请求报文，浪费服务器资源

![image-20220205143345421](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220205143345421.png)

#### 四次挥手

- 第一次挥手，客户端发送一个FIN报文，报文中指定一个序列号，客户端处于FIN_WAIT1状态，停止发送数据，等待服务端确认
- 第二次挥手，服务端收到FIN后，发送ACK报文，且将客户端报文的序列号+1作为ACK报文的序列号，服务端变为CLOSE_WAIT状态
- 第三次挥手，如果服务端也想断开，服务端也发送一个FIN报文，同样指定了一个序列号，服务端变为LAST_ACK状态
- 第四次挥手，客户端收到FIN，一样以ACK报文应答，变为TIME_WAIT状态，等待一段时间，确认服务端接收到自己的ACK报文，变成CLOSED状态；服务端只要接收到ACK，就变成CLOSED

第四次挥手中为什么要等待？

TIME_WAIT状态，等待两个传输时间，确保服务端已经收到自己的ACK报文了。如果服务端没收到，它会重发FIN报文

![image-20220205144050669](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220205144050669.png)

### HTTP常见请求头

| 字段名            | 说明                                                         | 示例                                                         |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Accept            | 能够接受的回应内容类型（Content-Types）                      | Accept: text/plain                                           |
| Accept-Charset    | 能够接受的字符集                                             | Accept-Charset: utf-8                                        |
| Accept-Encoding   | 能够接受的编码方式列表                                       | Accept-Encoding: gzip, deflate                               |
| Accept-Language   | 能够接受的回应内容的自然语言列表                             | Accept-Language: en-US                                       |
| Authorization     | 用于超文本传输协议的认证的认证信息                           | Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==            |
| Cache-Control     | 用来指定在这次的请求/响应链中的所有缓存机制 都必须 遵守的指令 | Cache-Control: no-cache                                      |
| Connection        | 该浏览器想要优先使用的连接类型                               | Connection: keep-alive Connection: Upgrade                   |
| Cookie            | 服务器通过 Set- Cookie （下文详述）发送的一个 超文本传输协议Cookie | Cookie: $Version=1; Skin=new;                                |
| Content-Length    | 以 八位字节数组 （8位的字节）表示的请求体的长度              | Content-Length: 348                                          |
| Content-Type      | 请求体的 多媒体类型                                          | Content-Type: application/x-www-form-urlencoded              |
| Date              | 发送该消息的日期和时间                                       | Date: Tue, 15 Nov 1994 08:12:31 GMT                          |
| Expect            | 表明客户端要求服务器做出特定的行为                           | Expect: 100-continue                                         |
| Host              | 服务器的域名(用于虚拟主机 )，以及服务器所监听的传输控制协议端口号 | Host: en.wikipedia.org:80 Host: en.wikipedia.org             |
| If-Match          | 仅当客户端提供的实体与服务器上对应的实体相匹配时，才进行对应的操作。主要作用时，用作像 PUT 这样的方法中，仅当从用户上次更新某个资源以来，该资源未被修改的情况下，才更新该资源 | If-Match: "737060cd8c284d8af7ad3082f209582d"                 |
| If-Modified-Since | 允许在对应的内容未被修改的情况下返回304未修改                | If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT             |
| If-None-Match     | 允许在对应的内容未被修改的情况下返回304未修改                | If-None-Match: "737060cd8c284d8af7ad3082f209582d"            |
| If-Range          | 如果该实体未被修改过，则向我发送我所缺少的那一个或多个部分；否则，发送整个新的实体 | If-Range: "737060cd8c284d8af7ad3082f209582d"                 |
| Range             | 仅请求某个实体的一部分                                       | Range: bytes=500-999                                         |
| User-Agent        | 浏览器的浏览器身份标识字符串                                 | User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:12.0) Gecko/20100101 Firefox/21.0 |
| Origin            | 发起一个针对 跨来源资源共享 的请求                           | Origin: http://www.example-social-network.com                |

### HTTP常见请求方法

- GET：向服务器请求数据
- POST：将实体提交到指定资源，通常造成服务器资源的修改
- PUT：上传文件，更新数据
- DELETE：删除服务器上的对象
- HEAD：获取报文首部，与GET相比，不返回报文的主体部分
- OPTIONS：询问支持的请求方法，用与跨域请求
- CONNECT：要求建立隧道，使用隧道进行TCP通信

### HTTP报文

#### 请求报文

- 请求行

  请求方法，URL，HTTP协议版本

- 请求头

  由关键字对组成，每行一对

- 空行

- 请求体

  POST和PUT等请求携带的数据

#### 响应报文

- 响应行

  网络协议，状态码，状态码原因

- 响应头

  关键字对组成，每行一对

- 空行

- 响应体

  服务器响应的数据

### HTTPS和HTTP

#### HTTP

**Hyper Text Transfer Protocol，超文本传输协议**。它传输的数据不是计算机底层的二进制包，而是完整的数据，如HTML文件、图片、查询结果等等，能够被上层应用所识别。以明文方式发送内容，不提供任何加密保护。

特点如下

- 支持客户/服务器模式
- 简单快速：客户只需向服务器传送请求方法和请求路径
- 灵活：允许传送任何类型的数据，数据类型由`Content-Type`标注
- 无连接：每次连接只处理一个请求（后面出现了`keep-alive`解决该问题）
- 无状态：无法根据之前的状态来对应地处理本次请求

#### HTTPS

HTTPS的出现是为了解决HTTP的不安全性

`HTTPS = HTTP + SSL/TLS`

![image-20220203103123020](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220203103123020.png)

流程如下

- 客户端通过URL访问服务端，建立SSL连接
- 服务端将证书信息传送给客户端
- 客户端和服务器开始协商SSL的安全等级
- 客户端的浏览器根据双方协商好的安全等级，建立会话密钥，用密钥对公钥加密，传回服务端
- 服务端用自己的私钥解密出会话密钥
- 服务端利用会话密钥加密和客户端的通信

#### 区别

- HTTPS更安全
- 默认端口不同：**HTTP是80，HTTPS是443**
- HTTPS需要进行加密和多次握手，性能不如HTTP
- HTTPS需要证书，证书功能越强大花费也越大

#### SSL加密

在HTTPS的加密中，采用的是对称加密+非对称加密，也就是混合加密

- 非对称加密解决密钥交换问题

  发送密文的一方使用对方的公钥进行加密处理，然后对方用自己的私钥拿来解密

- 数字证书

  数字证书认证机构处于客户端与服务器双方都可信赖的第三方机构的立场

  服务器的运营人员申请数字证书，机构会对已申请的公钥做一个数字签名，服务器在通信时就把这份数字证书也一起发给客户端

### HTTP版本

#### HTTP1.0

与服务器只保持短暂的连接，每次请求都需要和服务器建立一个TCP连接

想要建立长连接要手动设置`keep-alive`

#### HTTP1.1

到了1.1版本，默认支持长连接，建立一次连接，多次请求均由这个连接完成

引入了更多缓存控制策略

添加了一些新的请求方法，比如put，delete，options

#### HTTP2.0

- 多路复用

  HTTP2复用TCP连接，意味着在一个连接里，客户端和浏览器都可以同时发送多个请求或回应，且不用按照顺序一一对应，避免了队头堵塞

- 二进制分帧

  采用二进制格式传输数据，解析更高效

- 首部压缩

  在客户端和服务器之间使用首部表来跟踪和存储之前发送的键值对，对于相同的数据不再通过每次请求和响应发送

- 服务器推送

  引入了服务器推送，允许服务器推送资源给客户端

#### 总结

HTTP1.0：

- 浏览器与服务器只保持短暂的连接，浏览器的每次请求都需要与服务器建立一个TCP连接

HTTP1.1：

- 引入了持久连接，即TCP连接默认不关闭，可以被多个请求复用
- 在同一个TCP连接里面，客户端可以同时发送多个请求
- 虽然允许复用TCP连接，但是同一个TCP连接里面，所有的数据通信是按次序进行的，服务器只有处理完一个请求，才会接着处理下一个请求。如果前面的处理特别慢，后面就会有许多请求排队等着
- 新增了一些请求方法
- 新增了一些请求头和响应头

HTTP2.0：

- 采用二进制格式而非文本格式
- 完全多路复用，而非有序并阻塞的、只需一个连接即可实现并行
- 使用报头压缩，降低开销
- 服务器推送

### URL

#### 组成部分

以**http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name**为例

- 协议：该URL协议就是http，后面的`//`为分隔符
- 域名：该URL域名为`www.aspxfans.com`，一个URL中，也可以使用IP地址来作为域名
- 端口：跟在域名后的就是端口，域名和端口间用`:`作为分隔符。端口不是必须的，若省略，则为默认端口（80、443...）
- 虚拟目录：域名后的第一个'/'到最后一个'/'为止。也不是一个必须的部分
- 文件名：最后一个'/'到'?'为止。若没有'?'，就是到'#'。也不是必须的部分
- 参数：'?'到'#'，多个参数用'&'分隔
- 锚：'#'到最后，非必须

### UDP和TCP

![image-20220203103633891](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220203103633891.png)

- TCP面向连接，需要进行三次握手四次挥手，UDP不需要
- TCP提供可靠服务，采用流量控制、编号与确认、计时器等手段保证数据无差错；UDP尽可能地传输数据，但不保证数据完整性
- TCP面向字节流，会把应用层的报文分解为多个字节流，在目的站再重新装配；UDP直接发送应用层报文，接收方只需要去掉报文首部以后，原封不动地上交给上层应用
- TCP只能点对点全双工通信

![image-20220203103653476](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220203103653476.png)

TCP 应用场景适用于对效率要求低，对准确性要求高或者要求有链接的场景，而UDP 适用场景为对效率要求高，对准确性要求低的场景。

### 网络模型

![image-20220207101340034](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220207101340034.png)

- 应用层：DNS、HTTP、SMTP等等
- 传输层：TCP、UDP
- 网络层：IP层

### DNS

#### 是什么

Domain Names System，域名系统，是进行域名和相对应的IP地址的转换的服务器

#### 查询过程

- 首先搜索浏览器的DNS缓存，缓存中维护了一张域名与IP地址对应的表
- 若没有命中，则继续搜索操作系统的DNS缓存
- 若还是没有命中，操作系统就将域名发送到本地域名服务器，本地域名服务器递归查询自己的DNS缓存，成功则返回
- 若还没有命中，本地服务器就向上级服务器进行迭代查询
  - 首先本地域名服务器向根域名服务器发起请求，根域名服务器返回顶级域名服务器的地址给本地服务器
  - 本地域名服务器拿到这个顶级域名服务器的地址后，就向其发起请求，获取权限域名服务器的地址
  - 本地域名服务器根据权限域名服务器的地址向其发起请求，最终得到该域名对应的 IP 地址
- 本地域名服务器将IP地址返回给操作系统，同时加入自己的缓存
- 操作系统返回给浏览器，同时加入自己的缓存
- 浏览器得到IP地址，同时缓存

#### 协议

DNS占用53端口，同时使用UDP和TCP

- 在区域传输的时候使用TCP
- 在域名解析的时候使用UDP

### CDN

**Content Delivery Network，内容分发网络，简单来说，CDN就是根据用户的位置来分配最近的资源**

用户在上网时就无需访问源站，而只需要访问最近的一个CDN节点，也叫**边缘节点**，实质上就是一个缓存了源站资源的代理服务器

![image-20220203104510398](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220203104510398.png)

![image-20220203105033021](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220203105033021.png)

### 状态码

- 1 表示请求已被接收，需要继续处理，属于临时响应，只包含状态行和某些响应头信息
  - 100：通知客户端部分请求已被服务器接收，应当继续发送剩余部分
  - 101：服务器根据客户端的请求切换协议，主要用于websocket或http升级
- 2 表示成功
  - 200：成功，请求所希望的响应头或数据体将随此响应返回
  - 201：已创建，请求成功且服务器创建了新的资源
  - 204：无内容，服务器成功处理请求但没有返回任何内容
  - 206：部分内容，服务器成功处理了部分请求
- 3 表示重定向
  - 301：永久移动，请求的资源已经永久移动到新位置
  - 302：临时移动，以后仍继续用原来地址请求
  - 305：告知请求者应使用代理
- 4 表示请求错误
  - 400：错误请求，语法错误
  - 401：未授权，请求要求身份验证
  - 403：禁止
  - 404：找不到请求的资源
  - 405：方法被禁用
  - 408：请求超时
- 5 表示服务器错误
  - 500：服务器内部错误
  - 502：错误网关
  - 503：服务不可用，一般是超载或者停机维护
  - 505：HTTP版本不支持

### GET和POST

GET方法请求一个指定资源，该方法只被用于获取数据

POST方法用于将实体提交到指定的资源，会导致服务器的状态变化或副作用

#### 区别

- GET在浏览器回退时是无害的，但POST会再次提交请求
- GET请求会被浏览器主动缓存，POST则需要手动设置
- GET请求只能进行URL编码，POST支持多种编码方式
- GET请求的请求参数会被完整地保留在浏览器历史记录，POST则不会
- GET请求在URL中传送的参数有长度限制，POST没有
- GET只接受ASCII字符，POST没有限制
- GET的参数直接暴露在URL上，更不安全，不能拿来传递敏感信息，POST的参数放在Request body中
- 对于GET请求，浏览器会把`http header`和`data`一起发送，服务器响应200
- 对于POST，浏览器先发送`header`，服务器响应100，再发送`data`，服务器响应200

### 输入URL回车后发生什么

简单来说，发生以下几步

- URL解析

  ![image-20220205144812329](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220205144812329.png)

- DNS查询

  ![image-20220205144824063](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220205144824063.png)

- TCP连接

  通过DNS查询确定了IP地址后，经历三次握手建立TCP连接

- HTTP请求

  在已经建立了的TCP连接的基础上进行通信，浏览器发送HTTP请求到目标服务器

- 响应请求

  服务器响应客户端的请求，发回资源

- 页面渲染

### WebSocket

HTML5提供的一种浏览器与服务器进行全双工通信的网络技术，属于应用层。

基于TCP传输协议，并复用HTTP的握手通道，浏览器和服务器只需要一次握手，就可以创建持久性的连接，进行双向数据传输

最大特点：**服务器可以主动向客户端推送消息**

原理：客户端向服务器notify一个带有接收者ID(recipients IDs)的event，服务器接收后通知**所有**active的客户端，只有ID在recipients IDs序列中的客户端才会处理这个事件

- 支持双向通信，有更强的实时性
- 支持多种类型的数据
- 建立在TCP协议之上，服务端的实现较为容易
- 数据格式轻量，性能开销小
- **没有同源限制**，客户端可以和任何服务器通信
- 与HTTP有良好兼容性

## CSS

### 单位

| CSS单位      |                                        |
| ------------ | -------------------------------------- |
| 相对长度单位 | em、ex、ch、rem、vw、vh、vmin、vmax、% |
| 绝对长度单位 | cm、mm、in、px、pt、pc                 |

- px

  表示像素，每个像素点都是大小相同的

- em

  相对于当前对象内文本的字体尺寸，默认`1em = 16px`

  em会继承父级元素的字体大小

- rem

  相对于根元素`font-size`的值，常用于做自适应、移动端适配

- vh，vw

  把窗口高宽分为100等分

### 选择器

#### 优先级

内联 > ID选择器 > 类选择器 > 标签选择器

1000   100             10              1

#### 继承属性

- 字体属性
- 文本属性
- 元素可见性
- 表格布局
- 列表属性
- 引用属性
- 光标

特殊：a标签的字体颜色无法继承，h标签字体大小无法继承

#### 无继承属性

- display
- 盒子的属性
- 背景属性
- 定位属性
- 轮廓属性
- 生成内容属性
- 页面样式属性

### 盒模型

定义：当对一个文档进行布局的时候，浏览器的渲染引擎会根据标准之一的CSS基础框盒模型，将所有的元素表示为一个个的矩形盒子。

盒子模型分为两种

- W3C标准盒模型
- IE怪异盒模型

#### 标准盒模型

width和height只包含内容宽高，不包含padding和border

![image-20220203192749826](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220203192749826.png)

#### 怪异盒模型

![image-20220203192915536](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220203192915536.png)

width和height除了内容宽高，还包括了padding和border

### BFC

**Blocking Formatting Context，块级格式化上下文**，是页面中的一块渲染区域，拥有自己独立的渲染规则。

BFC的目的就是形成一个相对与外界**完全独立**的空间

- 内部的盒子会在垂直方向上一个接一个地放置
- 对于同一个BFC，两个相邻盒子的margin会发生塌陷，与方向无关
- 计算BFC高度时，浮动的子元素也参与计算
- 每个元素的左外边距与包含块的左边界相接触
- BFC的区域不会与float的元素区域重叠
- 容器里的子元素不会影响到外面的元素，反之亦然

#### 触发条件

- 根元素，即HTML元素
- 浮动元素
- overflow属性不为visible
- display的值为inline-block、inltable-cell、table-caption、table、inline-table、flex、inline-flex、grid、inline-grid
- position的值为absolute或fixed

#### 应用场景

- 防止margin塌陷

  简单来说，如果两个相邻的元素发生了margin的塌陷，就在其中一个元素外包裹一层容器，并使该容器触发BFC，那么这两个相邻的元素就不属于同一个BFC，也就不会出现外边距塌陷的情况了

- 清除内部浮动

  想要计算浮动元素的高度，就需要触发BFC

- 自适应多栏布局

#### 触发BFC

一般情况下，要触发BFC，只需修改overflow属性

```css
overflow: hidden;
```

### 居中

#### 水平居中

对于行内元素： `text-align: center;`

对于块级元素：

```css
// 自动边距
{
    margin: 0 auto;
}
// flex布局
{
    display: flex;
    justify-content: center;
}
// 绝对定位
{
    position: absolute;
    left: 50%;
    transform: translate(-50%, 0);
}
```

#### 垂直居中

对于行内元素：

```css
.parent {
	height: h;
}
.child {
    line-height: h;
}
```

对于块级元素：

- 绝对定位

  ```css
  {
  	position: absolute;
      top: 50%;
      transform: translate(0, -50%);
  }
  {
      position: absolute;
      top: 0;
      bottom: 0;
      margin: auto 0;
  }
  {
      position: absolute;
      top: 50%;
      height: h;
      margin-top: -0.5h;
  }
  ```

- flex

  ```css
  {
  	display: flex;
      align-items: center;
  }
  ```

### flex布局

`Flexible Box`，简称`flex`，意为弹性布局

容器中存在两条轴，主轴和交叉轴，二者垂直。容器中的元素默认沿主轴排列，可以通过`flex-direction`来决定主轴，默认是row

##### 属性

```css
.container {
    display: flex;
    flex-direction: row | row-reverse | column | column-reverse;
}
```

![image-20220225144314631](/images/%E5%A4%87%E6%88%98%E6%9A%91%E6%9C%9F%E5%AE%9E%E4%B9%A0/image-20220225144314631.png)

```css
.container {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```

`justify-content`：项目在主轴上的对齐方式

```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

`align-items`：项目在交叉轴上的对齐方式

```css
.container {
	align-items: flex-start | flex-end | center | baseline | stretch;
}
```

复合属性flex

`flex`属性是`flex-grow`，`flex-shrink`，`flex-basis`的简写，默认为`0 1 auto`



### CSS画三角形

原理：边框实际上是个梯形



### 性能优化

- 内联首屏关键CSS

  内联关键CSS，可以使浏览器下载完html后就立即渲染，使渲染时间提前。

  较大的CSS文件就不应该使用内联

- 异步加载CSS

- 资源压缩

  用webpack、rollup等模块化工具，压缩CSS代码

- 合理使用选择器

  不要过多嵌套选择器。因为CSS匹配规则是从右往左，先找到xx，排除祖先不是xx，再以此类推

  id选择器可以不再嵌套其他的

  通配符和属性选择器避免使用

- 减少使用昂贵的属性

  box-shadow，border-radius等等

- 不要使用@import

- 对可以继承的属性不要重复编写

- 小的icon图片转为base64编码

### 回流、重绘

#### 回流

布局引擎根据各种样式，计算盒子在页面上的**大小和位置**

#### 重绘

计算好位置和大小后，根据盒子特性进行**绘制**

#### 渲染过程

- 解析HTML，生成DOM树，解析CSS，生成CSSOM树
- 将DOM树和CSSOM树结合，生成渲染树(Render Tree)
- Layout(回流):根据生成的渲染树，进行回流(Layout)，得到节点的几何信息（位置，大小）
- Painting(重绘):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
- Display:将像素发送给GPU，展示在页面上

### 隐藏页面元素

- display设置为none

  元素彻底消失，会触发回流和重绘，不占据空间不触发事件

- visibility设置为hidden

  元素不可见，仍占据页面空间，不会触发回流，无法响应点击事件

- opacity设置为0

  元素不可见，仍占据页面空间，不会触发回流，仍可以响应点击事件

- height/width设置为0

  元素不可见，会触发回流和重绘，不占据空间不触发事件

- position设置为absolute，将其移出可视区域

- `transform: scale(0,0);`，将元素缩放为0，仍占据位置，但不会响应绑定的监听事件

### link和@import的区别

- link是XHTML标签，除了加载CSS，还可以定义RSS等其他事务；@import只能加载CSS
- link是在页面载入时同时加载，@import在页面完全载入后加载
- link无兼容问题
- link支持使用JS控制DOM去改变样式，@import不支持

### 两栏布局和三栏布局

#### 两栏布局

将页面分割成左右宽度不等的两列，宽度较小的列（次要布局元素）设为固定宽度，另一列（主要布局元素）填充满剩余空间

方法一，浮动

- 使用float，左浮左边栏
- 右边栏使用margin-left撑开固定的宽度
- 为父级元素添加BFC

方法二，flex布局

该方法要注意的是，flex布局中，align-items属性的默认值是stretch，导致了列等高的效果，若想让两个盒子高度自动，需要设置`align-items: flex-start;`

```html
<style>
    .box {
        display: flex;
    }
    .left {
        width: 100px;
    }
    .right {
        flex: 1;
    }
</style>
<div class="box">
    <div class="left">左边栏</div>
    <div class="right">主要内容区</div>
</div>
```

#### 三栏布局

三栏布局有很多种实现方法

方法一，两边float，中间margin撑开。该方法需要注意的是，html结构中，中间部分需要放在最后，否则右侧将会被挤到下方。同样，由于利用了浮动，父级元素需要添加BFC。这种方式存在缺陷：主体内容最后加载

方法二，两边absolute，中间margin。需要注意父元素需要用相对定位。

方法三，flex布局。类似于上述两栏布局。

#### 双飞翼



#### 圣杯

注意点：

- 要先渲染middle，middle写前面
- 理解负边距的作用

```html
<div class="header">header</div>
    <div class="content">
        <div class="middle">middle</div>
        <div class="left">left</div>
        <div class="right">right</div>
    </div>
<div class="footer">footer</div>
```

```css
.header {
    width: 100%;
    height: 30px;
    background: red;
}

.content {
    overflow: hidden;
    padding: 0 100px;
}

.footer {
    width: 100%;
    height: 30px;
    background: red;
}

.middle {
    position: relative;
    width: 100%;
    float: left;
    height: 100px;
    background: rebeccapurple;
}

.left {
    position: relative;
    width: 100px;
    height: 100px;
    float: left;
    left: -100px;
    margin-left: -100%;
    background: steelblue;
}

.right {
    position: relative;
    width: 100px;
    height: 100px;
    float: left;
    right: -100px;
    margin-left: -100px;
    background: steelblue;
}
```



### 动画

#### transition

#### animation

#### 二者区别

- transition是过渡属性，强调的是**过渡**，**需要一个触发事件**，比如焦点、点击等，才执行动画。只需设置开始帧和结束帧
- animation是动画属性，**设定好时间后可以自己执行**，且可以循环一个动画，可以设置多个关键帧(@keyframe)

### position

#### static

默认值，没有定位，出现在正常文档流，会忽略z-index或top等声明

#### relative

相对定位，**相对其原来的位置进行定位**，位置通过top等属性规定

#### absolute

绝对定位，相对于static定位以外的第一个父元素进行定位

#### fixed

固定定位，指定元素相对于屏幕可视窗口的位置来指定元素位置，位置在滚动时不会改变

#### sticky

粘性定位，依赖于用户的滚动，在relative和fixed间切换

#### fixed和absolute的区别

- 根元素不同：fixed的根元素是浏览器，absolute的是第一个非static的父元素
- absolute会滚动，fixed不会



## 浏览器

### 浏览器内部多个标签页通信

实现多标签页通信，本质上都是通过中介者模式

- websocket：标签页向服务器发数据，服务器向其他标签页转发
- localStorage：对localStroage进行监听
- postMessage
- ShareWorker

### 浏览器安全

#### XSS

#### CSRF

#### 网络劫持

##### DNS劫持

- DNS强制解析：通过修改运营商的本地DNS记录，引导用户流量到缓存服务器
- 302：通过监控网络出口的流量，分析判断哪些内容是否可以劫持处理；对劫持的内存发起302跳转的回复，引导用户获取内容

##### HTTP劫持

由于http明文传输，所以响应内容可能被修改(添加广告)

##### 解决

DNS劫持涉嫌违法，已被监管；http劫持就全站https，运营商无法获取明文，就无法劫持响应内容

### 路由

#### 为什么要使用路由

现在都使用AJAX异步请求完成页面的无缝刷新，导致请求时浏览器的URL不会有任何变化

#### hash模式

利用`window.location.hash`属性以及窗口的`onhashchange`事件，监听浏览器的hash地址变化，执行相应的js动态切换网页内容

- hash指地址中#号以及后面的内容，也称散列值，锚点
- hash不随着请求发到服务器端，所以改变hash不会重载
- 监听窗口的`onhashchange`事件，通过`window.location.hash`属性来获取和设置hash值
- hash变化会直接反映到浏览器地址栏

#### history模式

- URL中没有#，使用传统的路由分发模式：用户输入一个URL时，服务器会接收这个请求，并解析URL，然后做出相应的逻辑处理

- window.history指向History对象，代表当前窗口的浏览历史
- 可以通过`history.length`得出当前的窗口一共访问过几个网址
- 出于安全，浏览器不允许脚本读取这些地址，但允许地址之间导航
- 前进和后退其实就是对History对象进行操作

history模式有一个缺点，就是当改变页面地址后强制刷新浏览器，如果后端没有做好相应的准备就会报错404。因为刷新后，是拿当前的地址去请求服务器的，如果服务器没有相应的资源，就会返回404

如何解决这个缺点

- nginx代理
- 如果是vue项目，增加一个简单的回退路由，如果URL不匹配任何静态资源，服务器就提供与`index.html`相同的页面资源。比如使用firebase模拟后端时，修改`firebase.json`文件即可

### 缓存

#### 缓存位置

一共三种，按优先级排序

1. Service Worker：运行在主线程之外，缓存是持续性的
2. Memory Cache：内存缓存，效率最高，但持续性短，随着内存的释放而释放
3. Disk Cache：读取速度慢，但容量和时效性优

#### 强缓存

使用强缓策略时，只要判断有效就直接使用，不需要向服务器确认

- Expires

  - 服务器在响应头添加Expires属性，指定资源的过期时间

  - 使用绝对时间，在这个时间前都可以直接使用该缓存

- Cache-Control

  - no-cache，表示每次请求都要校验缓存是否过期
  - max-age，表示多久内不用重新请求资源
  - 优先级高于Expires

#### 协商缓存

如果缓存里的资源是新鲜的，则返回304，否则返回200

- Last-Modified / If-Modified-Since 
- Etag / If-None-Match

#### 不同的刷新方式

- 刷新按钮/F5：服务器对文件检查新鲜度
- Ctrl+F5：相当于清除了缓存
- 地址栏回车：正常流程

#### 本地存储

本地存储共有三种方式，`coookie`，`localStorage`和`sessionStorage`

#### 三者的异同

| 特性           | cookie                                                       | localStorage                                                | sessionStorage                                              |
| -------------- | ------------------------------------------------------------ | ----------------------------------------------------------- | ----------------------------------------------------------- |
| 生命周期       | 一般由服务器端生成，会设置失效时间。若没有设置，则默认关闭浏览器时清除 | 除非手动清除，否则永久保存                                  | 仅在当前会话下有效，关闭页面或浏览器后被清除                |
| 存放数据大小   | 4K左右                                                       | 5M                                                          | 5M                                                          |
| 与服务器端通信 | 每次都会被携带在HTTP请求头部中，若使用cookie保存过多数据会带来性能问题 | 仅在客户端保存，不与服务器端进行通信                        | 仅在客户端保存，不与服务器端进行通信                        |
| 易用性         | 需要程序员自己封装，源生的Cookie接口不友好                   | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 |

### 跨域

#### 来源

跨域问题的来源，在于浏览器为了安全引入的**同源策略**。

**同源**：协议、主机名、端口都相同

仅浏览器有跨域限制，服务端之间并不受影响

#### 同源策略限制了什么

主要目的在于保证用户的信息安全，只是对js脚本的限制，比如一般的img等请求都不会有这个限制，因为这些操作不会通过响应结果来进行可能出现安全问题的操作

- 当前域下的js脚本不能访问其他域的cookie、localStorage和indexDB
- 当前域下的js脚本不能操作、访问其他域下的DOM
- 当前域下ajax无法发送跨域请求

#### 如何判定

![image-20220203141604979](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220203141604979.png)

#### 解决方案

1. CORS

   CORS是目前最主流的跨域解决方案，方案依赖服务端在响应头添加`Access-Control-Allow-*`头，告知浏览器端应通过此请求。优势在于前端并不需要作出改动

   CORS将请求分为**简单请求**和**需预检请求**

   **简单请求**：请求方法为`GET, HEAD, POST`，请求头为`Accept`,`Accept-Language`,`Content-Language`,`Content-Type`。

   对于简单请求，浏览器就直接发出CORS请求，在请求的头信息中增加一个Origin字段，说明本次请求来自哪个源，服务器根据这个值来决定是否同意这次请求

   如果同意，返回的响应就会多出一些信息头；不同意，会返回一个正常的HTTP响应，但是上面没有`Access-Control-Allow-Origin`等信息。该错误无法通过状态码识别（可能还是200）

   **预检请求**：当一个请求不满足简单请求的条件时，浏览器就自动向服务端发出一个**OPTIONS请求**，通过服务端响应判定请求是否被允许

   请求头除了包含Origin字段，还会包含所用的方法字段以及将会额外发送的头信息字段。服务器判断这些字段后决定是否通过

   在CORS请求中，如果想要传递Cookie，就需要满足以下条件

   - 设置`withCredentials`为true
   - 设置`Access-Control-Allow-Credentials`为true
   - 设置`Access-Control-Allow-Origin`为非`*****`
   
2. 反向代理

   依赖同源的服务端做一个**转发处理**，将跨域请求转换为同源请求

   前端只需要切换接口

3. JSONP

   利用浏览器加载JavaScript资源文件时不受同源策略限制，即`<script>`标签没有跨域限制，而实现跨域获取数据，具体流程如下

   - 全局注册一个函数
   - 构造一个请求URL
   - 生成一个`<script>`并把`src`设置为上一步的URL，插入到文档中
   - 服务端构造一个JavaScript函数调用表达式并返回
   - 浏览器加载并执行以上代码

   JSONP的缺点

   - 只支持GET
   - 不安全，易受XSS攻击

4. Post Message

   即在两个 origin 下分别部署一套页面 A 与 B，A 页面通过 `iframe` 加载 B 页面并监听消息，B 页面发送消息。

5. windouw.name

   主要是利用 `window.name` 页面跳转不改变的特性实现跨域，即 `iframe` 加载一个跨域页面，设置 `window.name`，跳转到同域页面，可以通过 `$('iframe').contentWindow.name` 拿到跨域页面的数据。
   
6. nginx代理跨域

   实质和CORS原理一样
   
7. WebSocket



## Webpack

### 什么是Webpack

**一个用于现代JavaScript应用程序的静态模块打包工具**

静态模块，指的是开发阶段可以被webpack直接引用的资源，即可以直接被获取打包进`bundle.js`的资源

在webpack处理应用程序的时候，会在内部构建一个依赖图，依赖图对应映射到项目所需的每个模块，不再局限于js文件

### Webpack能做什么

- 编译代码能力，提高效率，提高兼容性

  ![image-20220220194420025](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220220194420025.png)

- 模块整合能力，提高性能和可维护性，解决频繁请求文件问题

  ![image-20220220194515198](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220220194515198.png)

- 模块化能力，项目维护性增强，支持不同种类的前端模块类型，统一的模块化方案，所有资源的加载都可以通过代码控制



## 手写JS系列

### instanceof

```js
function myInstanceof (target, Fn) {
    if(target === null || typeof target !== 'object') {
        return false
    }
    let proto = target.__proto__
    let prototype = Fn.prototype
    while(1) {
        if(proto === prototype) {
            return true
        }
        if(proto === null) {
            return false
        }
        proto = proto.__proto__
    }
}
```

### new

#### new都做了些什么？

- 创建一个空对象
- 为创建出的空对象添加属性`__proto__`，把该属性链接到构造函数的原型对象
- 将该对象作为this的上下文
- 如果该函数没有返回对象，则返回this

```js
function objectFactory () {
    let obj = new Object() // 创建空对象
    Constructor = [].shift.call(arguments) // 构造函数
    obj.__proto__ = Constructor.prototype // 链接
    let res = Constructor.apply(obj, arguments)
    return typeof res === 'object' ? res : obj
}
```



### `setTimeout`实现`setInterval`

```js
function mySetInterval(fn, interval) {
  // 设置递归函数，模拟定时器执行。
  function callback() {
    fn();
    setTimeout(callback, interval);
  }
  // 启动定时器
  setTimeout(callback, interval);
}
```



### bind，call和apply

#### call

call()方法在使用一个指定的this值和若干个指定的参数值的前提下，调用某个函数或方法

```js
Function.prototype.myCall = function (context) {
    context = context || window
    context.fn = this
    let args = [...arguments].slice(1)
    let res = context.fn(...args)
    delete context.fn
    return res
}
```

#### apply

```js
Function.prototype.myApply = function (context, arr) {
    context = context || window
    context.fn = this
    let res = null
    if (!arr) {
        res = context.fn()
    }
    else {
        res = context.fn(...arr)
    }
    delete context.fn
    return res
}
```

#### bind

bind()方法会创建一个新函数，当这个新函数被调用时，bind()的第一个参数会作为运行时的this，之后的一系列参数将会在传递的实参前传入作为它的参数

```js
Function.prototype.myBind = function (context) {
    if(typeof this !== 'function') {
        throw new Error('Not callable')
    }
    let self = this
    let args = [...arguments].slice(1)
    let fNOP = function () {}
    let fBound = function () {
        let bindArgs = Array.prototype.slice.call(arguments)
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs))
    }
    fNOP.prototype = this.prototype
    fBound.prototype = new fNOP()
    return fBound
}
```

### 防抖和节流

#### 防抖

n秒后才执行该事件，如果n秒内重复触发，则重新计时

```js
function debounce(func, wait) {
    let timeout = null
    return function () {
        let context = this // 保留上下文
        let args = arguments // 保留参数
        // 若已存在定时器，则需要重置定时器
        if(timeout) {
	        clearTimeout(timeout)
            timeout = null
        }
        // 设置定时器，在指定时间后执行函数
        timeout = setTimeout(() => {
            func.apply(context, args)
        }, wait)
    }
}
```

#### 节流

n秒内只运行一次，若n秒内重复触发也没用

```js
function throttle(func, delay) {
    let curTime = Date.now() // 记录当前时间戳
    return function() {
        let context = this
        let args = arguments
        let nowTime = Date.now()
        // 超过delay则执行函数
        if(nowTime - curTime >= delay) {
            curTime = Date.now()
            return func.apply(context, args)
        }
    }
}
```

#### 应用场景

1. 防抖，主要应用在**连续的事件，但只需要触发一次的场景**
   1. 搜索框的输入：用户停止输入后才显示建议
   2. 输入的校验
   3. 窗口大小的调整，只有当调整完成后，才计算新的大小并渲染
   4. 文本编辑器实时保存，如飞书、notion
2. 节流，**间隔一段时间执行一次回调**
   1. 滚动，每隔一秒计算一次位置信息
   2. 播放进度条
   3. 搜索联想（也可以做成防抖）

#### 要点

防抖，重在防止抖动。所以n秒内重复触发需要**清零**，代码关键在于**clearTimeout**

节流，重在控制流量。所以n秒内只可以触发一次，重在开关锁，timer = timeOut 或者 = null

### 浅拷贝

如果是基本数据类型，就直接拷贝值；如果是引用数据类型，直接拷贝内存地址

1. `Object.assign()`

   - 接受的第一个参数是目标对象，其余参数是源对象
   - 同名属性会覆盖
   - 若第一个参数不是对象，则会先把它转为对象，所以第一个参数不能是null或undefined

2. 扩展运算符

   ```js
   let cloneObj = {...obj}
   ```

3. 数组浅拷贝

   1. slice不加参数，可以实现数组浅拷贝
   2. concat不加参数，可以实现数组浅拷贝

4. 手写一个浅拷贝

   ```js
   function shadowCopy(obj) {
       if(!obj || typeof obj !== 'object') {
           return
       }
       let cloneObj = Array.isArray(obj) ? [] : {}
       for(let key in obj) {
           if(obj.hasOwnProperty(key))
       		cloneObj[key] = object[key]
       }
       return cloneObj
   }
   ```

### 深拷贝

与浅拷贝不同，对于一个新的引用类型，深拷贝会先创建一个引用类型，然后将相应的值赋给它，意味着内存地址不同

1. `JSON.stringify()`

   原理就是利用stringify将对象序列化，再用parse反序列化

   但是拷贝的对象如果含有函数，symbol或者undefined等，拷贝时它们会消失

   ```js
   let obj2 = JSON.parse(JSON.stringify(obj1))
   ```

2. 手写一个深拷贝

   ```js
   function deepCopy(obj) {
       if(!obj || typeof obj !== 'object') {
           return
       }
       let cloneObj = Array.isArray(obj) ? [] : {}
       for(let key of obj) {
           if(obj.hasOwnProperty(key))
           	cloneObj[key] =  typeof obj[key] === 'object' ? deepCopy(obj[key]) : obj[key]
       }
       return cloneObj
   }
   ```



### 数组的各种方法

#### push

```js
Array.prototype.myPush = function() {
    for(let i = 0; i < arguments.length; i++) {
        this[this.length] = arguments[i]
    }
    // push方法返回push后的长度
    return this.length
}
```

#### map

```js
Array.prototpe.myMap = function (func) {
    if(typeof func !== 'function') {
        throw new Error('Type Error')
    }
    // map会返回一个新数组
    let res = []
    for(let i = 0; i < this.length; i++) {
        res.push(func(this[i]))
        // 如果是forEach
        // this[i] = func(this[i])
    }
    return res
}
```

#### filter

```js
Array.prototype._filter = function(func) {
    if (typeof func !== "function") {
        throw Error('Type Error');
    }
    let res = [];
    for (let i = 0, len = this.length; i < len; i++) {
        func(this[i]) && res.push(this[i]);
    }
    return res;
}
```

### 数组拍平

先总结`Array.prototype.flat()`的特性和用法

- 返回的是新数组，对原数组没有影响
- 不传参时默认拉平一层，可以传整数表明要拉平的层数，Infinity为完全拉平，小于等于0为不拉平
- 会跳过空位

要实现一个数组拍平函数，要做到的是以下事情

- 遍历数组
- 判断数组元素是否是数组
- 如果是数组，拉平一层

可以根据这个思路，开始手写出第一版代码

```js
function myFlat (arr) {
	let arrRes = [] // 存放拍平的数组
    arr.map(item => {
        if(Array.isArray(item)) {
            arrRes = arrRes.concat(myFlat(item))
            // arrRes = arrRes.concat([].concat(...item))
        }
        else {
            arrRes.push(item)
        }
    })
}
```

该版本中，使用递归调用/扩展运算符的方法，实现拍平数组，但完成的是Array.prototype.flat(Infinity)，会把任何数组拍平至一维。要想达到通过参数控制的效果，需要进一步控制递归条件

```js
function myFlat(arr, depth) {
	let res = []
    arr.map(item => {
        Array.isArray(item) && depth > 1 ?
            res = res.concat(myFlat(item, depth - 1)) :
        	res.push(item)
    })
    return res
}
```



## JavaScript

### Promise

#### Promise是什么

Promise是异步编程的一种解决方案，ES6将其写进了语言标准，统一了用法

所谓Promise，简单来说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

从语法上，Promise是一个对象，可以从它获取异步操作的消息

#### Promise对象的特点

1. 对象状态不受外界影响。Promise对象有三个状态，pending，fulfilled，rejected。只有异步操作的结果可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise名称的由来：承诺
2. 一旦状态改变，就不可再变，任何时候都可以拿到这个结果。Promise的状态改变只有两种可能：pending到fulfilled或rejected。只要这两种情况发生，就称为定型，resolved

#### 优缺点

##### 优点

可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数，解决了回调地狱

可以支持多个并发的请求，获取并发请求中的数据

提供统一的接口，使得控制异步操作更容易

##### 缺点

无法取消Promise，一旦新建它就会立即执行，无法中途取消

如果不设置回调函数，内部抛出的错误不会反映到外部

处于pending状态时，无法得知目前进展到了哪个阶段

#### 基本用法

ES6规定，Promise对象是一个构造函数，用来生成Promise实例

Promise接收一个函数作为参数，该函数的两个参数分别是resolve和reject

resolve函数的作用：将Promise的状态变成fulfilled，在异步操作成功时调用，并将异步操作的结果作为参数传递出去

reject函数的作用：将Promise的状态变成rejected，在异步操作失败时调用，并将报出的错误传递出去

```js
const myPromise = new Promise(function(resolve, reject) {
    if(/* 异步操作成功 */) {
    	resolve(value)   
    }
    else {
        reject(error)
    }
})
```

Promise实例生成以后，可以用then方法分别指定fulfilled和rejected状态的回调函数

then方法接收两个回调函数作为参数，第一个回调函数

```js
myPromise.then(function(value) {
    // success
}, function(error) {
    // failure
})
```

#### 手写基本的Promise

```js
class myPromise {
    // 定义三种状态
    static PENDING = '准备态'
    static FULFILLED = '完成态'
    static REJECTED = '拒绝态'

    constructor(func) {
        // 默认状态
        this.status = myPromise.PENDING

        // 执行结果
        this.result = null

        // 保存回调函数
        this.resolveCallbacks = []
        this.rejectCallbacks = []

        try {
            // 绑定this再调用
            func(this.resolve.bind(this), this.reject.bind(this))
        } catch (error) {
            // 报错的话要reject
            this.reject(error)
        }
    }

    resolve(result) {
        setTimeout(() => {
            if (this.status === myPromise.PENDING) {
                this.status = myPromise.FULFILLED
                this.result = result
                this.resolveCallbacks.forEach(callback => {
                    callback(result)
                })
            }
        });
    }

    reject(result) {
        setTimeout(() => {
            if (this.status === myPromise.PENDING) {
                this.status = myPromise.REJECTED
                this.result = result
                this.rejectCallbacks.forEach(callback => {
                    callback(result)
                })
            }
        });
    }

    then(onFULFILLED, onREJECTED) {
        // 为了实现链式调用
        // then返回一个promise实例
        return new myPromise((resolve, reject) => {
            // then传入的两个参数需要是函数
            onFULFILLED = typeof onFULFILLED === 'function' ? onFULFILLED : () => { }
            onREJECTED = typeof onREJECTED === 'function' ? onREJECTED : () => { }

            if (this.status === myPromise.PENDING) {
                this.resolveCallbacks.push(onFULFILLED)
                this.rejectCallbacks.push(onREJECTED)
            }

            if (this.status === myPromise.FULFILLED) {
                setTimeout(() => {
                    onFULFILLED(this.result)
                });
            }

            if (this.status === myPromise.REJECTED) {
                setTimeout(() => {
                    onREJECTED(this.result)
                });
            }
        })
    }
}

const promise = new myPromise((resolve, reject) => {
    resolve('成功')
})

promise.then(
    resolve => { console.log(resolve) },
    reject => { console.log(reject) }
)
```



### 数据类型

#### 基本类型

- number
- string
- boolean
- undefined
- null
- symbol

#### Symbol

Symbol 是一种特殊的、不可变的数据类型，可以作为对象属性的标识符使用

##### 作用

- 元编程，更改原生js的行为
- 作为key使用，用`Reflect.ownKeys()`拿到所用key属性

#### 引用类型

复杂类型统称`Object`

- Object
- Array
- Function
- Date
- RegExp
- Map
- Set

#### 判断数据类型的方法

##### `typeof`

`typeof`返回一个字符串，表示未经计算的操作数的类型

可以用来判断一个变量是否存在，即`typeof`是不是为`undefined`

缺点：对于引用类型，只有function能被识别，其他都只能识别为object

##### `instanceof`

`instanceof`用于检测构造函数的prototype属性是否出现在某个实例对象的原型链上

缺点：不能正确判断基本数据类型

##### `Object.prototype.toString.call`

统一返回`[object xxx]`格式的字符串

##### 通用检测方法

结合以上方法，可以写出通用的数据检测方法

```js
function getType(obj) {
    let type = typeof obj
    // 基本数据类型可以直接返回
    if(type !== 'object') {
        return type
    }
    // 正则处理
    return Object.prototype.toString.call(obj).replace(/^\[object (\S+)\]$/, '$1')
}
```



### 原型和原型链



### 闭包

**闭包，指有权访问另一个函数作用域中变量的函数**

创建闭包最常见的方式是在函数内创建另一个函数，创建的新函数可以访问到当前函数的局部变量

无论何时何地，如果将函数作为返回值，就会看到闭包在这些函数中的应用，例如防抖节流就是经典的闭包

#### 用途

- 使我们在函数外部可以访问到函数内部的变量，可以创建私有变量
- 使已经运行结束的函数上下文中的变量继续保留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收

### 作用域和作用域链

#### 作用域

##### 全局作用域

- 最外层函数和最外层函数外面定义的变量拥有全局作用域
- 未定义而直接赋值的变量拥有全局作用域
- 所有window对象的属性拥有全局作用域
- 过多全局作用域变量会污染全局命名空间，引起命名冲突

##### 函数作用域

- 声明在函数内部的变量
- 内层作用域可以访问外层作用域，反之不行

##### 块级作用域

- let和const声明或由大括号包裹的代码片段
- 循环中适合绑定块级作用域

#### 作用域链

在当前作用域中查找所需变量，但该作用域没有这个变量，则它就是自由变量，就需要依次向上级作用域查找，直到访问到window对象，一层层的关系就是作用域链

**保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，可以访问到外层环境的变量和函数**

作用域链的本质就是一个**指向变量对象的指针列表**，变量对象是一个包含了执行环境中所有变量和函数的对象。

作用域链的前端始终是当前执行上下文的对象。后端始终是全局对象

### 事件模型

#### 定义

JS中的事件，可以理解为在HTML文档或者浏览器中发生的一种交互操作，使得网页具备交互性，比如加载事件，鼠标事件等等

#### 事件流

事件流都会经历三个阶段

- 事件捕获
- 处于目标
- 事件冒泡

![image-20220202230628414](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220202230628414.png)

#### 三种模型

- 原始事件模型
  - 直接绑定（HTML或JS），绑定速度快
  - 只支持冒泡，不支持捕获
  - 同一个类型的事件只可以绑定一次。后绑定的会覆盖先绑定的。
- 标准事件模型
  - 共有三个过程，事件捕获阶段：事件从`document`一直往下传到目标元素，依次检查经过的节点是否绑定了事件监听函数，有则执行；事件处理阶段：事件到达目标节点并触发监听函数；事件冒泡阶段：事件从目标元素一直往上传到`document`，依次检查经过的节点是否绑定了事件监听函数，有则执行。
  - `addEventListener(eventType, handler, useCapture)` useCapture用于指定是否在捕获阶段进行处理，默认为false
- IE事件模型（基本已废弃）
  - 只有两个阶段：事件处理和事件冒泡
  - `attachEvent(eventType, handler)`

#### 如何阻止事件冒泡

- `event.stopPropagation()`



### 事件代理

通俗地来说，事件代理，就是把一个元素响应事件的函数委托给另一个元素，事件委托在冒泡阶段完成。

事件委托把一个或一组元素的事件委托到它的父节点或者更外层的节点上，真正绑定事件的成了外层元素

#### 优点

- 减少需要的内存，提升整体性能
- 动态绑定，减少重复的工作

#### 局限性

- `focus`, `blur`等没有冒泡机制，无法进行委托
- `mousemove`, `mouseout`等事件，虽然有冒泡阶段，但只能通过不断通过位置去计算定位，对性能消耗高，也不适合事件委托
- 如果全用事件代理，可能会出现事件误判

#### 应用

有一个列表，有大量表项，需要在点击表项时响应一个事件

如果要为每个表项都绑定上一个事件，**内存消耗非常大**

```js
const lis = document.getElementByTagName('li')
for (const li of lis) {
    li.onClick = function (e) {
        console.log(e.target.innerHTML)
    }
}
```

这时就可以利用事件代理，把事件绑定在ul元素上，执行事件的时候再去匹配目标元素

```js
document.getElementById('ul').addEventListener('click', (e) => {
    // 兼容性处理
    const event = e || window.event
    const target = event.target || event.srcElement
    // 判断是否匹配目标元素
    if(target.nodeName.toLocalLowerCase === 'li') {
        console.log(target.innerHTML)
    }
}
```



### 事件循环

#### 事件循环是什么

JS是一个单线程的语言，实现单线程非阻塞的方式就是事件循环

在JS中，所有的任务都可以分为同步任务和异步任务，同步任务将直接进入主线程执行，异步任务推入任务队列。当主线程的任务清空，就去任务队列读取任务

#### 宏任务和微任务

- 宏任务：script脚本的执行，setTimeout，setInterval，setImmediate
- 微任务：promise的回调，node中的nextTick等

#### Event Loop

- 执行同步任务
- 执行完所有同步任务，执行栈为空，检查是否有异步任务
- 执行所有微任务
- 开始下一轮Event Loop，执行宏任务中的异步代码



### 变量提升与函数提升

包括变量和函数在内的所有声明都在代码被执行前首先处理(仅针对var)

- 定义声明是在编译阶段进行的
- 赋值声明会被留在原地等待执行阶段

**JavaScript 引擎并不总是按照代码的顺序来进行解析。在编译阶段，无论作用域中的声明出现在什么地方，都将在代码本身被执行前首先进行处理，这个过程被称为提升。声明本身会被提升，而包括函数表达式的赋值在内的赋值操作并不会提升。**

#### 变量提升

使用var声明的变量存在变量提升

```js
name = 'xyh'
var name
console.log(name) // xyh
```

上面这个例子就很好地体现了变量提升

从上往下看，我们先添加了全局变量name，window.name = 'xyh'

然后我们再次声明了name

最后我们打印出的结果却不是未初始化的undefined而是xyh

ES6新增的const和let可以规避变量提升

#### 函数提升

函数字面量不会被提升

#### 优先级

函数提升的优先级高于变量提升



### 数组的遍历方法

| **方法**                  | **是否改变原数组**   | **特点**                                                     |
| ------------------------- | -------------------- | ------------------------------------------------------------ |
| forEach()                 | 取决于元素的数据类型 | 数组方法，没有返回值，是否会改变原数组取决于数组元素的类型是基本类型还是引用类型 |
| map()                     | 否                   | 数组方法，不改变原数组，有返回值，可链式调用                 |
| filter()                  | 否                   | 数组方法，过滤数组，返回包含符合条件的元素的数组，可链式调用 |
| for...of                  | 否                   | for...of遍历具有Iterator迭代器的对象的属性，返回的是数组的元素、对象的属性值，不能遍历普通的obj对象，将异步循环变成同步循环 |
| every() 和 some()         | 否                   | 数组方法，some()只要有一个是true，便返回true；而every()只要有一个是false，便返回false. |
| find() 和 findIndex()     | 否                   | 数组方法，find()返回的是第一个符合条件的值；findIndex()返回的是第一个返回条件的值的索引值 |
| reduce() 和 reduceRight() | 否                   | 数组方法，reduce()对数组正序操作；reduceRight()对数组逆序操作 |

### 数字精度丢失

为什么在JavaScript中，`0.1 + 0.2 !== 0.3`？

这是因为，在JavaScript中，是用浮点数的格式来存储数字的，采用的是`IEEE754`规范，这样的存储结构的优点是可以归一化处理整数和小数，节省存储空间

计算时要从十进制转到二进制，而比如0.1，转成二进制是除不尽的，由于精度的问题就会出现误差

#### 解决方案

```js
function strip(num, precision) {
    return +parseFloat(num.toPrecision(precision))
}
```

### this指向

绝大多数情况下，函数的调用方式决定了this的指向

在函数执行过程中，this一旦被确定了就不可以再更改

#### 绑定规则

##### 默认绑定

非严格模式下，this默认绑定到全局对象

##### 隐式绑定

函数可以作为某个对象的方法来调用，此时this自然就指向**上一级对象**

##### new绑定

通过new生成一个实例对象，此时this就指向这个实例对象

##### 显示修改

apply，call，bind可以显示修改this的指向

#### 箭头函数

ES6中的箭头函数，让我们能在代码书写的时候就确定好this的指向，即编译时绑定this



### ES6

#### var，let和const

- 使用var声明的变量存在变量提升

- 使用var可以对一个变量进行多次声明

- 函数内使用var，则该变量是局部的

- 暂时性死区：只要块级作用域里存在let、const命令，这个区域就不再受外部影响：在let或const声明该变量前，它都是不可用的。**只有等到声明变量的那一行代码出现，才可以获取和使用该变量**

  ```js
  var a = 123
  if(true) {
  	a = 'ABC' // Reference Error
  	let a
  }
  ```

- const必须初始化，且变量值不能再修改



#### Set和Map

共同点：都可以存储不重复的值

不同点：集合Set以[值, 值]方式存储，字典Map以[键, 值]方式存储

##### Set

类似于数组，但是成员的值是唯一的

常见方法

- add()，添加某个值，返回Set本身
- delete()，删除某个值，返回一个布尔值表示是否成功删除
- has()，判断是否包含某元素
- clear()，清楚成员



#### for...in 和 for...of 的区别

for...of是ES6新增的遍历方式，允许遍历一个含有iterator接口的数据结构，比如对象、数组，并且返回各项的值，和ES3中的for...in区别如下

- for...of遍历获取的是对象的键值value，for...in获取的是键名key
- for...in会遍历对象的整个原型链，性能很差不推荐使用
- 对于数组的遍历，for...in会返回数组中所有可枚举的属性，for...of只会返回下标对应的属性值
- 一般使用for...of



#### 箭头函数

- 更加简洁
- 没有自己的this，只会在自己的上一层作用域继承this，所以箭头函数中的this在定义时就确定，之后不会改变，即使使用call和apply
- 不能作为构造函数使用
- 没有自己的arguments
- 没有prototype
- 不能作为Generator函数



#### Proxy

定义：用于定义基本操作的自定义行为

本质：修改程序默认行为，相当于在编程语言层面上修改，属于元编程

##### 用法：

Proxy为构造函数，它用来生成Proxy实例

`let proxy = new Proxy(target, handler)`

target表示的是要拦截的目标对象，它可以是一个任何类型的对象，也可以是另一个代理

handler以函数作为属性的对象，各属性中的函数分别定义了执行各种操作时代理的行为



## Typescript

### 是什么

- TypeScript是JavaScript的超集，支持ES6语法，支持面向对象编程的概念。
- 它是一种静态类型检查的语言，提供了类型注解，在代码编译阶段就可以检查出数据类型的错误。
- 扩展了JavaScript的语法，现有的JavaScript程序可以不作改变地在TypeScript中工作。
- 是为大型应用开发设计的语言，编译阶段仍需要编译成纯JavaScript。

### 特性

- 类型批注和检查

- 类型推断

- 类型擦除：编译过程中批注的内容和接口会在运行时擦除

- 接口：定义数据的类型

  ```tsx
  interface Person {
  	name: string;
  	age: number;
  }
  let tom: Person = {
  	name: 'Tom',
  	age: 25
  }
  ```

- 枚举：用于取值被限定在一定范围内的场景

- Mixin：可以接受任意类型的值

- 泛型：写代码时使用一些以后才指定的类型

- 名字空间：名字只在该区域有效，其他区域可以重复使用该名字

- 元组：合并不同类型的对象，相当于一个可以装不同类型数据的数组

### 数据类型

- boolean
- number
- string
- array
  - `let arr: string[] = ['ss', 'pp']`
  - `let arr: Array<number> = [1, 2]`
- tuple：元组类型
  - `let tupleArr: [number, string, boolen] = [12., '34', false]`
- enum
- any
- null, undefined
- void，表示该方法没有返回值
- never，是其他类型的子类型，可以赋值给任何类型，代表从来不会出现的值。一般来指定那些总是会抛出异常、无限循环。返回never的函数必须存在无法达到的终点
- object

### type和interface



## React

### React特性

#### JSX语法

#### 单向数据绑定

#### 虚拟DOM

#### 声明式编程

#### 组件



### 对Hooks的理解

Hook是React16.8的新特性，可以在不编写class的情况下使用state以及其他的React特性

#### 引入目的

- 解决难以重用和共享的组件中的与状态相关的逻辑
- 逻辑复杂的组件难以开发与维护，每个生命周期函数可能会包含互不相关的逻辑在里面
- 类组件的this增加学习成本

#### 常见Hooks

##### `useState`

在函数组件内部通过`useState`来实现函数内部维护state

##### `useEffect`

用于在函数组件内部进行一些带有副作用的操作

第一个参数接受一个回调函数，默认情况下，会在第一次渲染和更新后都执行

第二个参数是用来跳过某些不必要的回调，仅在第二个参数改变时才执行回调

##### `useContext`

Context Object是被`createContext()`这个API所建立的，内部有两个子元件：Provider和Consumer

- Provider：传递Value
- Consumer：接收Value

![image-20220428122629696](/images/%E5%A4%87%E6%88%98%E6%9A%91%E6%9C%9F%E5%AE%9E%E4%B9%A0/image-20220428122629696.png)

![image-20220428122655812](/images/%E5%A4%87%E6%88%98%E6%9A%91%E6%9C%9F%E5%AE%9E%E4%B9%A0/image-20220428122655812.png)

##### `useCallback`

```js
useCallback(fn, deps) === useMemo(() => fn, deps)
```



##### `useMemo`

仅在某个依赖项改变时才重新计算，有助于避免在每次渲染时都进行高开销的计算

要记住，在传入`useMemo`的函数会在渲染期间执行，所以不能在这个函数内部执行与渲染无关的操作



### React事件机制

React基于浏览器的事件机制，自身实现了一套事件机制，被称为合成事件

#### 合成事件

合成事件是React模拟原生DOM事件所有能力的一个事件对象，即浏览器原生事件的跨浏览器包装器

```js
<button onclick="handleClick()">按钮命名</button>

const button = <button onClick={handleClick}>按钮命名</button>
```

- React上注册的事件最终会绑定在document这个DOM上，而不是React组件所对应的DOM，这一做法减少了内存开销
- 自身实现了一套事件冒泡机制
- 通过队列的形式，从触发的组件向父组件回溯，然后调用他们JSX中定义的callback



### React生命周期

#### 创建阶段



#### 更新阶段

#### 卸载阶段

### React组件通信

#### 父组件向子组件传递

由于单向数据绑定的机制，父组件向子组件传递数据是最常见的方式

父组件在调用子组件时直接在标签内传递参数，子组件就能通过`props`接收

#### 子组件向父组件传递

父组件向子组件传递一个函数，然后通过这个函数的回调，拿到子组件传来的值

#### 兄弟组件传值

父组件作为中间层

#### 父组件向后代组件传值

利用`useContext`这个Hook

#### 非关系组件传值

全局管理，使用`redux` 或者`valtio`这种库

#### 总结

由于React是单向数据流，它的核心思想是**组件不会改变接收的数据，只会监听数据的变化**。当数据变化时它们就使用接收到的新的值，而不是去修改已有的值

因此，可以看到通信过程中，数据的存储位置都是存放在父级





### 受控组件和非受控组件

#### 受控组件

组件的**状态全程响应外部数据**

需要一个初始状态，和一个状态更新事件函数

#### 非受控组件

一般是在初始化的时候接受外部数据，然后自己在内部存储自身状态，当需要的时候，可以用`ref`查询`DOM`并查找当前值

#### 应用场景

| 特征                   | 受控               | 非受控             |
| ---------------------- | ------------------ | ------------------ |
| 一次性取值，例如submit | :white_check_mark: | :white_check_mark: |
| 提交时验证             | :white_check_mark: | :white_check_mark: |
| 即时现场验证           | :white_check_mark: | :x:                |
| 有条件地禁用按钮       | :white_check_mark: | :x:                |
| 强制输入格式           | :white_check_mark: | :x:                |
| 一个数据的多个输入     | :white_check_mark: | :x:                |
| 动态输入               | :white_check_mark: | :x:                |



### React引入CSS的方式和区别

#### 直接使用

直接在组件内书写CSS样式，通过style属性直接引入

优点：内联样式，样式间不会有冲突；可以动态获取当前state的状态

缺点：大量的样式会导致代码混乱；无法编写复杂样式，比如伪类伪元素

#### 组件引入CSS文件

将CSS单独写在一个文件中，然后组件直接引入

缺点：样式全局生效，部分样式之间可能会相互影响

#### 组件引入`.module.css`文件

将CSS文件作为一个模块引入，这个模块中的CSS只作用于当前组件，不会影响后代组件

#### CSS in JS





## Vue

### Vue3和Vue2的区别

#### `Proxy`替代`defineProperty`

##### `Object.defineProperty()`

该方法直接在对象上定义一个新属性或者修改现有属性，返回该对象

实现响应式的途径是通过get和set两个属性

缺点：

- 检测不到对象属性的新增和删除
- 数组的一些方法无法监听到
- 需要对每个属性遍历监听，如果是嵌套对象需要深层监听，带来性能问题

##### `Proxy`

监听是针对一个对象的，对这个对象的所有操作都会进入监听，就可以完整代理所有属性了

缺点：兼容性差

#### `Composition API`替代`Options API`

##### `Options API`



##### `Composition API`



##### 优势

- 逻辑复用
- 类型推断
- 代码更易压缩
- 减少this指向不明的情况

#### 性能提升

#### Treeshaking



### 双向数据绑定

![image-20220210203027755](E:/myBlog/static/images/%E5%85%AB%E8%82%A1/image-20220210203027755.png)

Vue采用的是**基于数据劫持的双向数据绑定**

- 利用`Proxy`或者`Objcet.defineProperty`等方法，生成Observer对对象/对象属性进行劫持，**在属性发生变化后就可以通知订阅者**
- 解析器Compiler解析模板中的指令Directive，**收集指令所依赖的方法和数据**，等待数据变化后渲染
- Watcher属于Observer和Compiler的桥梁。**将接收到的Observer产生的数据变化，根据Compiler提供的指令，来进行视图的渲染，使得数据变化反映在视图变化上**

#### `Object.defineProperty`

在Vue2中，数据劫持用的是`Object.defineProperty`。它的作用就是劫持对象的属性。通常对属性的getter和setter进行劫持，就能够在属性变化时进行特定的相应的操作。

缺点：对某些属性进行操作时无法拦截。比如对于数组而言，大多操作都无法拦截，除非自己手动重写数组的方法

#### Proxy

Vue3中，改用Proxy来进行数据劫持



### MVVM

#### 定义

MVVM：Model，View，ViewModel

- Model代表数据模型，数据和业务逻辑都在Model里定义
- View代表UI视图，负责数据的展示
- ViewModel负责监听Model的数据变化并控制视图的更新，处理用户的交互操作

Model和View并无直接关联，Model和ViewModel之间有数据双向绑定的关系。MVVM框架使得开发者只需要专注于数据的维护操作，而不需要自己去操作DOM

#### 优点

- 分离了视图和模型，降低代码耦合度，提高重用性。一个ViewModel可以绑定许多不同的view
- 提高可测性。ViewModel的存在可以帮助开发者更好地编写测试代码
- 自动更新DOM。利用双向绑定，数据更新后视图就会自动更新，不需要手动操作DOM

#### 缺点

- 不好调试。看到界面发生异常时，不知道是View还是Model出了问题。双向数据绑定还会使一个位置的bug传递到其他位置，更难定位出原始为止。数据绑定的声明是指令式地写在View中，这些内容无法打断点debug
- Model过大。模块很大时，如果不释放内存就会占用过高内存，降低性能
- 维护成本高

#### Vue中的MVVM

MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，**通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁**，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。



### 为什么data是函数而不是对象

一句话，**函数返回的对象内存地址并不相同**

vue组件可能会有多个实例，采用函数可以返回一个全新的data，每个实例的数据就不会被其他实例的污染



### 组件通信

- props传递数据
  - 适用场景：父组件传递数据给子组件
  - 子组件设置一个props属性，定义接收父组件传来的参数
  - 父组件在子组件标签中使用字面量来传值
- $emit触发自定义事件
  - 适用场景：子组件传递数据给父组件
  - 子组件通过$emit触发自定义事件，第一个参数为事件名，第二个参数为传递的值
  - 父组件绑定监听器，拿到传递的值
- EventBus
  - 适用场景：兄弟组件传值
  - 创建一个中央事件总线
  - 兄弟组件通过`$emit`触发自定义事件，通过`$on`监听自定义事件
- vuex
  - 适用场景：复杂关系的组件数据传递
  - 相当于一个用来存储共享变量的容器



### v-show和v-if

#### 共同点

作用效果相同，都是控制元素是否在页面上显示

#### 区别

- 控制手段不同

  v-show隐藏元素时，为该元素添加了`display: none`，dom元素依旧存在；

  v-if则是直接添加或删除这个dom

- 编译过程不同

  v-if切换有一个局部编译/卸载的过程，v-show只是基于css切换

- 编译条件不同

  v-if是真正的条件渲染，它会确保在切换过程中条件块内的事件监听器和子组件适当地被销

- 生命周期不同

  v-if由false变为true的时候，触发组件的`beforeCreate`、`create`、`beforeMount`、`mounted`钩子，由true变为false的时候触发组件的`beforeDestory`、`destoryed`

  v-show切换时，不会触发组件的生命周期

- 性能消耗不同

  v-if有更高的切换消耗，v-show有更高的初始渲染消耗（因为不管初始条件是什么，v-show控制的元素都会被渲染）

- 使用场景不同

  非常频繁地切换，使用v-show；条件很少改变，就用v-if

### computed和watch

#### computed

computed是计算属性的，会根据所依赖的数据动态显示最新的计算结果，该结果将会被缓存起来。当依赖的数据发生改变，才重新调用getter来计算最新的结果。设计初衷是为了使模板中的逻辑运算更简单：**复杂的计算逻辑放到computed中去计算**

##### 应用场景

- 重复使用数据或者复杂费时的运算
- 若需要的数据依赖于其他数据，可以设计为computed中

##### 和methods的区别

- computed是响应式的，只有依赖发生改变才重新求值，而methods每次调用都会重新求值，不是响应式的
- computed中的成员可以只定义一个函数作为只读属性, 也可以定义成 get/set变成可读写属性, 但是methods中的成员没有这样的

#### watch

watch是一个对data的数据监听回调，当依赖的data的数据变化，会执行回调，回调中会传入`newVal`和`oldVal`两个参数

Vue实例会在实例化的时候调用`$watch()`，遍历watch对象的每一个属性

##### 使用场景

当data中的某个数据变化时，需要做一些操作；或是需要在数据变化时执行异步或者开销大的操作

#### 二者异同

相同点

- 都是观察页面数据变化的

不同点

- computed只有当依赖数据变化时才计算，没变化就直接读取缓存
- watch每次都要执行函数，更适合用于数据变化时的异步操作。更多的是**起到观察的作用**



### 插槽Slot

**Slot是Vue的内容分发机制**，**组件内部的模板引擎使用Slot元素作为承载分发内容的出口**



### 保存当前页面的状态

#### 组件将被卸载的情况

- 保存在`localStorage`或者`sessionStorage`

  在组件被销毁前，将当前组件的state通过`JSON.stringify()`保存下来

  优点：

  - 兼容性好
  - 简单快捷

  缺点：

  - 状态是通过`JSON.stringify()`保存的，相当于是深拷贝，如果状态里有特殊情况，比如Date、RegExp等，就会得到字符串而非原来的值

- 路由传值

#### 组件不会被卸载

- 单页面渲染

  要切换的组件作为子组件全屏渲染，父组件正常储存页面状态

### Vue3和Vue2的区别



### 生命周期

- `beforeCreate`：数据观测和初始化事件都没有开始，此时无法访问到data，computed，watch和methods上的数据
- `created`：实例创建完成，但此时渲染的节点还没有挂载到DOM，无法访问`$el`属性
- `beforeMount`：在挂载前被调用，render函数首次调用。模板编译完成，根据data的数据和模板生成html
- `mounted`：用上一步编译好的html来替换el指向的DOM对象，渲染到html页面。此过程中进行ajax交互
- `beforeUpdate`：响应式数据更新时调用，此时虽然数据更新了，但是真实DOM还没有被渲染
- `updated`：DOM已经根据响应式数据的变化更新了。应该避免在此期间更改状态。**服务器端渲染期间不可调用**
- `beforeDestroy`：销毁前调用，这一步实例仍然完全可用
- `destroyed`：调用后，Vue实例指示的所有东西会被解绑，所有事件监听被移除，所有子实例被销毁。**服务器端渲染期间不可调用**
- 对于keep-alive包裹的组件，有独有的生命周期`activated`和`deactivated`，切换时不会销毁，而是缓存到内存中

#### 生命周期顺序

**加载渲染过程：**

​	1.父组件`beforeCreate`

​	2.父组件 `created`

​	3.父组件 `beforeMount`

​	4.子组件 `beforeCreate`

​	5.子组件 `created`

​	6.子组件 `beforeMount`

​	7.子组件 `mounted`

​	8.父组件 `mounted`

**更新过程：**

1. 父组件 `beforeUpdate`
2. 子组件 `beforeUpdate`
3. 子组件 `updated`
4. 父组件 `updated`

**销毁过程：**

1. 父组件 `beforeDestroy`
2. 子组件 `beforeDestroy`
3. 子组件 `destroyed`
4. 父组件 `destoryed`

#### 请求异步数据

推荐在created钩子函数中调用异步请求

- 可以更快地获取到服务端的数据，减少页面加载时间，提升用户体验
- SSR不支持`beforeMount`和`mounted`钩子函数，放在`created`中助于一致性



### v-for和v-if为什么不应连用

因为v-for的优先级比v-if高，所以如果将v-for和v-if应用在同一个元素上，每次渲染都会先循环，才进行条件判断，浪费大量性能

为避免出现这种情况，在v-for元素外再嵌套一层，在该层应用v-if

如果条件出现在循环内部，可以通过computed提前过滤那些不需要渲染的元素



### keep-alive

`keep-alive`是vue中的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染DOM

`keep-alive` 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们

它可以设置以下属性

- include：名称匹配则缓存
- exclude：名称匹配则不缓存
- max：最多缓存多少组件实例

基本用法

```vue
<keep-alive include="a, b">
    <component :is="view"></component>
</keep-alive>
```



### vue-router

#### vue-router中的懒加载

- 方法一，箭头函数+动态import（重点）

  ```js
  const List = () => import('@/components/list.vue')
  const router = new VueRouter({
    routes: [
      { path: '/list', component: List }
    ]
  })
  ```

- 方法二，箭头函数+动态require

  ```js
  const router = new Router({
    routes: [
     {
       path: '/list',
       component: resolve => require(['@/components/list'], resolve)
     }
    ]
  })
  ```

#### `$route`和`$router`的区别

- `$route`是**路由信息对象**，包括了path，params，hash，query，name等路由信息参数
- `$router`是**路由实例**，包括了路由的跳转方法和钩子函数

#### history模式

使用的是传统的路由分发模式

##### 特点

- URL没有#号，更好看
- 需要后台配置支持，否则可能返回404
- 会出现在HTTP请求中

##### 原理

#### hash模式

开发中默认的模式就是hash模式

##### 特点

- hash值会出现在URL里面，但不会出现在HTTP请求中，对后端完全没有影响。所以改变hash值不会重载页面
- 兼容性好，低版本的IE也支持
- 被称为前端路由，是SPA的标配

##### 原理

监听`onhashchange()`事件

```js
window.onhashchange = function (event) {
    console.log(event.oldURL, event.newURL)
    let hash = location.hash.slice(1)
}
```

hash值的变化对应的URL都会被浏览器记录下来，这样浏览器就能够实现页面的前进和后退

#### replace和push的区别

##### `this.$router.push()`

1. 跳转到指定URL，同时向URL栈添加一个新的记录，点击回退会返回上个页面

2. 参数可以是一个字符串路径，或者一个描述地址的对象

   ```js
   this.$router.push('/index')
   this.$router.push({path: '/index', query: {name: '123'}})
   ```

##### `this.$router.replace()`

1. 跳转到指定URL，替换history栈中最后一个记录，点击回退会返回至上上个页面

### `Vuex`

#### 是什么

`Vuex`就是一个专门为Vue应用开发的**状态管理模式**。

它的核心是store，也就是一个容器，包含着应用中的各种state，它的状态存储是响应式的

#### action和mutation的区别

mutation的操作，是一系列的同步函数，用来修改state的变量的状态。使用时需要通过commit来提交需要操作的内容。每个mutation都有一个字符串的事件类型(type)和一个回调函数(handler)，该回调函数就是实际上进行状态更改的地方，会接受state作为第一个参数

action类似于mutation，但仍有不同点

- action可以包含任意的异步操作
- action提交的是mutation，action无法直接变更状态

总结二者的区别

- mutation专注修改state，是修改state的唯一途径。action处理业务代码和异步请求
- 视图更新时，先触发action，再触发mutation
- mutation的参数是state，action则是context，是state的父级

#### `Vuex`和`localStorage`的区别

- `Vuex`储存在内存中，`localStorage`则以文件形式存储在本地。所以读取`vuex`中的数据比较快
- `Vuex`用于组件间共享状态，`localStorage`一般在跨页面传递数据时使用
- `Vuex`是响应式的
- 刷新时`Vuex`存储的值会丢失

#### `Vuex`和一个单纯的全局对象相比有什么优点

- `Vuex`状态存储是响应式的，当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新
- `Vuex`限制不能直接改变store中的状态，更方便追踪状态变化





## 操作系统

### 进程和线程

#### 进程

进程，是操作系统中最核心的概念，是对正在运行中的程序的一个抽象，是系统进行资源分配和调度的基本单位。

操作系统的所有其他内容都是围绕着进程展开的，负责执行这些任务的是CPU

#### 线程

线程是操作系统能够进行运算调度的最小单位，其是进程中的一个执行任务（控制单元），负责当前进程中程序的执行

#### 二者关系

- 一个进程至少有一个线程，一个进程可以运行多个线程
- 一个进程的线程之间共享同一块内存，可以共享对象和资源

#### 二者区别

- 本质：进程是操作系统资源分配的基本单位，线程是任务调度和执行的基本单位
- 开销：每个进程都有独立的代码和数据空间；线程可以看做轻量化的进程；进程切换的开销大
- 所处环境：操作系统中能同时运行多个进程，一个进程中有多个线程同时执行
- 内存分配：系统在运行时为每个进程分配不同的内存空间，线程所使用的资源来自所属进程的资源
- 包含关系：没有线程的进程可以看作是单线程的；线程是进程的一部分





## 关键思想

### 虚拟DOM

#### 什么是虚拟DOM

本质上来说，虚拟DOM其实就是一个JavaScript对象。将页面抽象成JS对象的形式，使跨平台渲染成为可能，同时还能有效减少页面渲染的次数，减少修改DOM的重绘重排次数

**虚拟DOM是对DOM的抽象**，是一个更加轻量级的对DOM的描述

#### 为什么要用虚拟DOM

- 保证性能下限，在不进行手动优化的情况下也提供较好的性能

  渲染时：

  - 真实DOM：生成HTML字符串，重建所有DOM元素
  - 虚拟DOM：生成vNode + Diff + 必要的DOM更新

- 跨平台

  本质就是JS对象，可以很方便地跨平台操作，比如服务端渲染等

#### 虚拟DOM性能

- 首次渲染大量DOM时，因为多了一层虚拟DOM的计算，会比较慢
- 只是保证了性能下限，如果对真实DOM操作注意优化，真实DOM性能上限更高

#### Diff算法原理

- 首先对比节点本身，判断是否为同一节点，如果不同，删除并创建新节点替换
- 如果是相同节点，进行patchVNode，判断怎么处理这个节点的子节点
- 如果新的没有子节点，旧的有，就直接移除
- 如果都有，就进行updateChildren，判断如何对他们的子节点操作
- 匹配时找到相同子节点就递归比较子节点
- 只对同层的子节点进行比较而放弃了跨级的比较



### AJAX

AJAX的全称，是**Asynchronous JavaScript and XML**，指的是通过JavaScript的异步通信，从服务器获取XML文档从中提取数据，再更新当前网页的对应部分，而不用刷新整个网页

#### 创建AJAX请求

- **创建一个`XMLHttpRequest`对象**
- 在该对象上**使用open方法创建一个HTTP请求**，所需参数有请求方法、请求地址、是否异步以及认证信息
- 发送请求前，可以为这个对象**添加一些信息以及监听函数**
- 对象的属性和监听函数设置完成后，最后**调用sent方法发送请求**



## 性能优化

### 图片懒加载

#### 方法一

- 拿到所有的图片DOM
- 遍历判断是否在可视区域内
- 若是，则设置图片的src属性
- 绑定window的scroll事件，对其进行监听（需要节流来控制scroll）

#### 方法二

有一个新的API，`IntersectionObserver`，可以自动观察元素是否可见，即交叉观察器

```js
var io = new IntersectionObserver(callback, option)

// 开始观察
io.observe(document.getElementById('example'))

// 停止观察
io.unobserve(element)

// 关闭观察器
io.disconnect()
```

### 分包加载



​    

## 自我介绍

面试官好，我是夏宇航，福建泉州人，来自浙江大学信息与电子工程学院，现在在读大三。

我的专业是信息工程，学校并不会教授前端方面的相关课程。我是从去年七月开始系统性地自学前端的，学习途径主要就是看网络上的教学视频，阅读一些技术书籍，以及自己写一些小项目来巩固学到的知识。

除了自己开发的小项目以外，我还有用过 `React Native` 开发应用的经历，是一个社交类别的应用，用户可以进行发帖、评论、追踪等操作，是一个功能完备的应用，我独立负责前端，另一位朋友负责后端，现在正在测试阶段，即将在 iOS 和 安卓两个平台都上线。

我还有过微信小游戏的开发经历，当时是和朋友五人组队参加了腾讯举办的高校小程序开发大赛，我们的小游戏 `Greenet` 获得了全国二等奖，期间开发、看着自己的程序一点点完善地展现出来的乐趣和成就感，也是我决定走上前端之路的主要原因之一。

实习经历的话，大三上学期时有过一段在有赞的实习，是在资产前端部门，做的工作是维护内部平台以及内部工具的开发，实习期间感觉得到了很多锻炼和提升，希望在下一段实习也能在提升、充实自己的同时，做一些有趣有意义的工作

以上就是我的自我介绍








