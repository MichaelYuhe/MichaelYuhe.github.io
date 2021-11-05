# 有赞前端实习_一面


## 有赞日常实习一面 11月2日

### 线上笔试

直播写代码

网站：`showMeTheCode`，有赞自己的平台

#### 1、字符串相加   

忘记处理最后一步的进位

```js
/**
* 给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和
* 
* tips: 你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式。
* 
* 示例输入: '197880986', '5112281222'
* 示例输出: '5310162208'
* 
*/
var addStrings = function (num1, num2) {
    // code here
    let len1 = num1.length, len2 = num2.length
    let carry = 0, sum = 0
    let res = []
    while (len1 || len2) {
        if (len1 && len2) {
            sum = parseInt(num1[--len1]) + parseInt(num2[--len2]) + carry
        }
        else if (len1) {
            sum = carry + parseInt(num1[--len1])
        }
        else {
            sum = carry + parseInt(num2[--len2])
        }
        carry = 0
        res.unshift(sum % 10)
        if (sum >= 10) {
            carry = 1
        }
    }
    return res.join('')
};

console.log('result --> ' + addStrings('197880986', '5112281222'));
console.log('want ----> ' + '5310162208');
console.log('[equal] -> ' + (addStrings('197880986', '5112281222') === '5310162208'));
```

#### 2、两个对象快速相等

```js
/**
 * fastEqual 两个对象快速相等
 * 
 * 示例输入: a = { arr: [1, 2], num: 12 } b = { arr: [1, 2], num: 12 }
 * 
 * 示例输出: true
 * 
 * tips: 不需要考虑太多边界情况, 优先保证执行效率, 输入 a / b 可以是任意数据类型
 */
function fastEqual(obj1, obj2) {
    // code here
    var o1 = obj1 instanceof Object;
    var o2 = obj2 instanceof Object;
    // 两个简单的判断
    if (!o1 || !o2){
        return obj1 === obj2
    }
    if(Object.keys(obj1).length !== Object.keys(obj2).length){
        return false;
    }
    // 循环看属性
    for(var o in obj1){
        var t1 = obj1[o] instanceof Object;
        var t2 = obj2[o] instanceof Object;
        // 如果都还是对象
        if(t1&&t2){
            if(!fastEqual(obj1[o],obj2[o])) {
                return false
            }
        }
        else if(obj1[o] !== obj2[o]){
            return false;
        }
    }
    return true;
}
```

#### 3、解析URL

没有解码，callback输出错误

```js
/**
 * 解析 URL 中的 Query String，返回一个对象
 */
// 返回值示例：
// {
//   name: 'coder',
//   age: '20'.
//   callback: 'https://youzan.com?name=test'
// }

const testURL = 'https://www.youzan.com?name=coder&age=20&callback=https%3A%2F%2Fyouzan.com%3Fname%3Dtest';

function parseQueryString(url) {
    // code here
    let res = {}
    // 分割
    if (url.indexOf('?') === -1) {
        return res
    }
    let query = url.split('?')[1].split('&')
    query.forEach((item) => {
        let temp = item.split('=')
        // 解码
        res[temp[0]] = decodeURIComponent(temp[1])
    })
    return res;
}

console.log("====== QueryString Parser ======")
console.log("should --> " + JSON.stringify({
    name: 'coder',
    age: '20',
    callback: 'https://youzan.com?name=test'
}));
console.log("[result] --> " + JSON.stringify(parseQueryString(testURL)));
```

**总结：三道题都大概做出来了，但是都不完美**

### 计算机网络

**XSS、CSRF**

讲讲非简单请求和简单请求

```js
// 简单请求：同时满足两个条件
// 请求头是 HEAD,POST,GET（讲漏了POST）
// 头信息不超出 Accept Accept-Language Content-Language Last-Event-ID  Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
// 非简单请求的CORS请求，会在通信之前增加一个预检请求
```

### 浏览器

跨域，具体用过哪些，上线了还可以`devServer`吗

### Vue

computed和watch的区别

vue-router的模式 ，**底层原理**

生命周期（父子组件间）

### CSS

position各种属性及其特点

居中 

### Javascript

基本数据类型有哪几种 ，**Symbol的具体作用** 

怎么判断是不是数组

```js
// 答出来的
Object.prototype.toString.call(arr)
Array.isArray(arr)
arr instanceof Array
// 其他的方法
arr.__proto__ === Array.prototype
arr.constructor === Array
```

async、await讲一下，**和forEach连用会发生什么**

### 其他

为什么选择前端

### 面试官的建议

学习网络相关的知识，包括网络安全

#### 


