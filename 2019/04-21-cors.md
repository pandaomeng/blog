---
title: 跨域知识点梳理
date: 2019-04-22
tag: [web]
---

# 跨域知识点梳理

## 问题

1. 什么是跨域和跨域资源共享（CORS）？两者有何关系？
2. 为什么会发生跨域？跨域会产生什么危险？
3. 跨域能不能被解决？
4. 在跨域的情况下，我们如何正确请求数据？
5. 在跨域的情况下，我们如何正确地进行dom查询？



## 一、什么是跨域和跨域资源共享（CORS）？两者的关系？

跨域：只要协议、域名、端口中有任何一个不同，都会被当作是不同的域（即不同的源），不同的域之间的请求就是跨域请求。

（CORS）是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。是跨域的一个解决方案。

MDN:

> 跨域资源共享(CORS) 是一种机制，它使用额外的 HTTP 头来告诉浏览器  让运行在一个 origin (domain) 上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域 HTTP 请求。

CORS 只是跨域的一种解决方案，其他的方案还有诸如（JSONP，代理等）

<!--more-->

## 二、为什么会发生跨域？跨域会产生什么危险？

参考 [不要再问我跨域的问题了](https://segmentfault.com/a/1190000015597029)

为什么会跨域？是谁在搞事情？为了找到这个问题的始作俑者，请点击[浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)。

答案是浏览器在搞事情，浏览器为了安全而使用了这样的安全机制（同源策略）。

浏览器是从两个方面去做这个同源策略的，一是针对接口的请求，二是针对Dom的查询。没有这样的限制上述两种动作有什么危险？

### 没有同源策略限制的接口请求

CSRF（Cross-site request forgery）跨站请求伪造，也被称为“One Click Attack”或者Session Riding。缩写为：CSRF/XSRF。

引用博客 [浅谈CSRF攻击方式](http://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html ) 的原理图：

![](https://images.pandaomeng.com/c8c0b71e9bfca82032e26f155cf84a32.jpg)

简单的来说，攻击者盗用了你的身份，以你的名义发送恶意请求。

从上图可以看出，要完成一次CSRF攻击，受害者必须依次完成两个步骤：

1.登录受信任网站A，并在本地生成Cookie。

2.在不登出A的情况下，访问危险网站B。2019/04-21-cors.md

### 没有同源策略限制的Dom查询

```
// HTML
<iframe name="yinhang" src="www.yinhang.com"></iframe>
// JS
// 由于没有同源策略的限制，钓鱼网站可以直接拿到别的网站的Dom
const iframe = window.frames['yinhang']
const node = iframe.document.getElementById('你输入账号密码的Input')
console.log(`拿到了这个${node}，我还拿不到你刚刚输入的账号密码吗`)
```

如果浏览器没有对Dom查询进行同源限制，那我们就可以随意取到iframe中（不同源）的内容。（比如可以偷窥聊天记录什么的）

**总结：跨域是浏览器为了安全做的一件事（同源策略）。**



## 三、跨域能不能被解决？

跨域是浏览器的固有行为，不能被解决。除非安装插件，例如chrome上的一款[插件](https://chrome.google.com/webstore/detail/access-control-allow-cred/hmcjjmkppmkpobeokkhgkecjlaobjldi)，可以屏蔽浏览器的这个安全策略。

但是我们可以解决跨域带来的“获取不到数据”的问题。下一节会详细讲。

上面讲到，跨域是浏览器为了安全做的一种限制。但是现在很多网站都是前后端分离的（前后端的域不同），跨域虽然防住了一些潜在危险，但同时也防住了正常的数据访问。



## 四、在跨域的情况下，我们如何正确请求数据？

前提：浏览器做了同源策略的限制，即跨域。（所以跨域只存在在浏览器上）

### JSONP

引wikipedia：

> **JSONP**（**JSON with Padding**）是数据格式[JSON](https://zh.wikipedia.org/wiki/JSON)的一种“使用模式”，可以让网页从别的网域要数据。另一个解决这个问题的新方法是[跨来源资源共享](https://zh.wikipedia.org/wiki/%E8%B7%A8%E4%BE%86%E6%BA%90%E8%B3%87%E6%BA%90%E5%85%B1%E4%BA%AB)。
>
> 由于[同源策略](https://zh.wikipedia.org/w/index.php?title=%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5&action=edit&redlink=1)，一般来说位于server1.example.com的网页无法与 server2.example.com的服务器沟通，而[HTML](https://zh.wikipedia.org/wiki/HTML)的 [\<script\>](https://zh.wikipedia.org/wiki/HTML%E5%85%83%E7%B4%A0#script_tag)元素是一个例外。利用 [\<script\>](https://zh.wikipedia.org/wiki/HTML%E5%85%83%E7%B4%A0#script_tag)元素的这个开放策略，网页可以得到从其他来源动态产生的JSON数据，而这种使用模式就是所谓的 JSONP。用JSONP抓到的数据并不是JSON，而是任意的JavaScript，用 JavaScript解释器运行而不是用JSON解析器解析。

简单的来说，就是script标签不会跨域，我们通过这个特性，在其src中填写请求的url，然后后端返会可直接执行的js，该js调用本地方法，参数就是json数据。

缺点：JSONP只能发起GET请求

### CORS

CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)。看名字就知道这是处理跨域问题的标准做法。CORS有两种请求，简单请求和非简单请求。

这里引用阮一峰老师的博客 [跨域资源共享 CORS 详解](<http://www.ruanyifeng.com/blog/2016/04/cors.html>)

> 浏览器将CORS请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。

### 代理

通过nginx配置，前端请求的时候还是使用前端的域名，到nginx的时候再转发到后端接口。

**Nginx配置**:

```
server{
    # 监听9099端口
    listen 9099;
    # 域名是localhost
    server_name localhost;
    #凡是localhost:9099/api这个样子的，都转发到真正的服务端地址http://localhost:9871 
    location ^~ /api {
        proxy_pass http://localhost:9871;
    }    
}
```



## 五、在跨域的情况下，我们如何正确地进行dom查询？

1. 使用postMessage和addEventListener在不同域间通信，即：由被查询的domain主动发起postMessage。

2. 主域名相同，但子域名不同的iframe跨域可以设置document.domain
3. canvas操作图片的跨域问题，参考张鑫旭老师的[解决canvas图片getImageData,toDataURL跨域问题](https://www.zhangxinxu.com/wordpress/2018/02/crossorigin-canvas-getimagedata-cors/)