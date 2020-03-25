---
title: HTTP协议与安全
date: 2019-05-06 11:16:48
tags:
---

#### HTTP
##### HTTP协议的主要特点
* 简单快速（每个资源是固定的url） 
* 灵活（通过头部type会有不同结果） 
* 无连接（连接一次就会断掉） 
* 无状态（客户端和服务端）
    
##### HTTP报文的组成部分
1. 请求报文
    * 请求行 （http方法 页面地址 http协议 http版本）
    * 请求头 （key val 值告诉服务端需要什么内容 注意类型）
    * 空行   （遇到空行时，下一个当着请求体）
    * 请求体
2. 响应报文（状态行，响应头，空行，响应体） 

##### HTTP 状态码
* 1xx:指示信息，请求已接收，继续处理
* 2xx 表示成功，请求已被成功接收
* 3xx 表示需要进一步操作，重定向，完成请求必须进行进一步的操作
* 4xx 表示浏览器方面出错，客户端错误，请求有语法错误或者请求无法实现
* 5xx 表示服务器方面出错，服务端错误，服务器未能实现合法的请求

##### 常见状态码
* 200 客户端请求成功
* 206 客户发送一个带有range(范围)头的GET请求，服务器完成它（用于视频）    
* 301 所请求的页面已经转移至新的url
* 302 所请求的页面已经临时转移至新的url
* 304 客户端有缓冲的文档并发出了一个条件性的请求，服务器告诉客户，原来缓冲的文档还可以继续使用
* 400 客户端请求有语法错误，不能被服务器所理解
* 401 请求未经授权，这个状态码必须和WWW-Authenticate报文域一起使用 
* 403 对被请求页面的访问被禁止
* 404 请求资源不存在
* 500 服务器发生不可预期的错误原来缓冲的文档还可以继续使用
* 503 请求未完成，服务器临时过载或当机，一段时间后可能恢复正常
<a href="http://www.runoob.com/http/http-status-codes.html" style="color: blue;">想了解更多</a> 

##### HTTP 缓存 (缓存就是资源文件在浏览器中存在的备份或者副本,如：网上请求的图片，去存在本地磁盘)
* 缓存分类
    * 强缓存(以下两个缓存都下发，以后者为准)
        * Expires Expires:THu,21 Jan 2017 23:39/;02 GMT（绝对时间，客服端和服务器时间不一致，比较的时候，是一浏览器时间，下发是服务器时间）
        * Cache-Control Cache-Control:max-age=3600（3600是相对时间）
    * 协商缓存（和服务器协商）
        * Last-Modified If-Modified-Since Last-Modified: Web, 26 Jan 2017 23:39/;02 GMT
        * Etag If-None-Match
    * 与浏览器缓存相关的http头有哪些，就是以上几个
* 总结：
    * ETag 是通过对比浏览器和服务器资源的特征值（如MD5）来决定是否要发送文件内容，如果一样就只发送 304（not modified）
    * Expires 是设置过期时间（绝对时间），但是如果用户的本地时间错乱了，可能会有问题
    * CacheControl: max-age=3600 是设置过期时长（相对时间），跟本地时间无关
<a href="https://imweb.io/topic/5795dcb6fb312541492eda8c" style="color: blue;">详细了解 ETag、CacheControl、Expires 的异同</a>

##### GET 和 POST 的区别
* 一般理解
    1. GET在浏览器回退时是无害的，而POST会再次提交请求
    2. GET产生的URL地址可以被加入收藏栏，而POST不可以
    3. GET请求会被浏览器主动cache，而POST不会，除非手动设置
    4. GET请求只能进行url编码，而POST支持多种编码方式
    5. GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留
    6. GET请求在URL中传送的参数是有长度限制的，而POST么有
    7. 对参数的数据类型，GET只接受ASCII字符，而POST没有限制
    8. GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息
    9. GET参数通过URL传递，POST放在Request body中
* 核心
    * 就一个区别：语义——GET 用于获取资源，POST 用于提交资源
<a href="https://zhuanlan.zhihu.com/p/22536382" style="color: blue;"> 请参考 </a>

##### Cookie V.S. LocalStorage V.S. SessionStorage V.S. Session
* Cookie V.S. LocalStorage
    * 主要区别是 Cookie 会被发送到服务器，而 LocalStorage 不会
    * Cookie 一般最大 4k，LocalStorage 可以用 5Mb 甚至 10Mb（各浏览器不同）
* LocalStorage V.S. SessionStorage
    * LocalStorage 一般不会自动过期（除非用户手动清除），而 SessionStorage 在会话结束时过期（如关闭浏览器）
* Cookie V.S. Session
    * Cookie 存在浏览器的文件里，Session 存在服务器的文件里
    * Session 是基于 Cookie 实现的，具体做法就是把 SessionID 存在 Cookie 里

##### XSS（Cross-Site Scripting）：跨越脚本攻击
* 原理
    * 不需要做任何的登录验证，核心原理向页面注入脚本
* 造成 XSS 有几个要点：
    1. 恶意用户可以提交内容
    2. 提交的内容可以显示在另一个用户的页面上
    3. 这些内容未经过滤，直接运行在另一个用户的页面上
* XSS 的成因以及防御措施
    1. 后台模板问题：要后台输出的时候，将可疑的符号 < 符号变成 &lt; （HTML实体）就行。
    2. 前端代码问题：就是不要自己拼 HTML，尽量使用 text 方法。如果一定要使用 HTML，就把可疑符号变成 HTML 实体

##### CSRF（Cross Site Request Forgery）：跨站请求伪造
* 原理
    * 攻击者构造网站后台某个功能接口的请求地址，诱导用户去点击或者用特殊方法让该请求地址自动加载。用户在登录状态下这个请求被服务端接收后会被误以为是用户合法的操作。对于 GET 形式的接口地址可轻易被攻击，对于 POST 形式的接口地址也不是百分百安全，攻击者可诱导用户进入带 Form 表单可用POST方式提交参数的页面。
* 使用anti-csrf-token的方案。 具体方案如下：
    1. 服务端在收到路由请求时，生成一个随机数，在渲染请求页面时把随机数埋入页面（一般埋入 form 表单内，<input type='hidden' name='_csrf_token' value='xxxx'>）
    2. 服务端设置setCookie，把该随机数作为cookie或者session种入用户浏览器
    3. 当用户发送 GET 或者 POST 请求时带上_csrf_token参数（对于 Form 表单直接提交即可，因为会自动把当前表单内所有的 input 提交给后台，包括_csrf_token）
    4. 后台在接受到请求后解析请求的cookie获取_csrf_token的值，然后和用户请求提交的_csrf_token做个比较，如果相等表示请求是合法的。
* 总结：
    1. CSRF
        * (网站中某个接口存在漏洞，用户在这个注册网站确实登录过) 
        * 防御措施 
            * Token验证：用户请求之后，会返回cookie信息，并且服务器向本地存储Token
            * Referer验证: 页面来源，服务器判断页面来源是否是当前站点下的
    2. xss: 向xss注入的js不可执行  
* 两者区别
    * xss向页面注入js运行，js函数体去做想做的
    * CSRF是利用本身的漏洞，帮你去执行相应的接口
* 推荐查阅资料
    1. <a href="https://zhuanlan.zhihu.com/p/22521378" style="color: blue;"> 文章 xsrf </a>
    2. <a href="https://zhuanlan.zhihu.com/p/22500730" style="color: blue;"> 文章 xss </a>
    3. <a href="http://www.imooc.com/learn/812" style="color: blue;"> 视频 csrf xss </a>

 

