---
title: 跨域
date: 2019-05-05 22:08:28
tags:
---

#### 跨域的几种解决方案
##### JSONP
<a href="https://zhuanlan.zhihu.com/p/22600501" style="color: blue;">来龙去脉啦</a>
* 同源策略(Same origin Policy)：浏览器出于安全方面的考虑，只允许与同域下的接口交互。
* 同域指的是？
    * 同协议：都是http或者https
    * 同域名：都是http://baidu.com/a 和http://baidu.com/b
    * 同端口：都是80端口
* JSONP的原理
    1. JSONP是通过 script 标签加载数据的方式去获取数据当做 JS 代码来执行
    2. 提前在页面上声明一个函数，函数名通过**接口传参**的方式传给后台，后台解析到函数名后在原始数据上「包裹」这个函数名，发送给前端。换句话说，JSONP 需要对应接口的后端的配合才能实现
```
// 简单的实现原理就是
 <script src="http://www.abc.com/?data=name&callback=jsonp" charset="utf-8"></script>
<script type="text/javascript">
    jsonp showData(res) {
        console.log(res);
    }
</script>
// 借用大佬封装的代码
function jsonp(setting) {
  setting.data = setting.data || {};
  setting.key = setting.key || 'callback';
  setting.callback = setting.callback || function() {};
  setting.data[setting.key] = '__onGetData__';

  window.__onGetData__ = function(data) {
    setting.callback (data);
  }
  var script = document.createElement('script');
  var query = [];
  for(var key in setting.data) {
    query.push( key + '=' + encodeURIComponent(setting.data[key]));
  }
  script.src = setting.url + '?' + query.join('&');
  document.head.appendChild(script);
  document.head.removeChild(script);
}
jsonp({
  url: 'http://api.baidu.com/weather.php',
  callback: function(res) {
    console.log(res)
  }
})
jsonp({
  url: 'http://photo.sina.cn/aj/index',
  key: 'jsoncallback',
  data: {
    page: 1,
    cate: 'recommend'
  },
  callback: function(res) {
    console.log(res)
  }
})
```

##### <a href="https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS" style="color: blue;">CORS</a>
<a href="http://www.ruanyifeng.com/blog/2016/04/cors.html" style="color: blue;">阮一峰教程</a>
* CORS需要浏览器和服务器同时支持
* 实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。
在header信息中添加Origin
CORS与JSONP的使用目的相同，但是比JSONP更强大。
JSONP只支持GET请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。

##### 闭包（Closure）
<a href="https://zhuanlan.zhihu.com/p/22486908" style="color: blue;">还是老师厉害</a>
* **「函数」和「函数内部能访问到的变量」（也叫环境）的总和，就是一个闭包**
* 平时我们的认知的闭包:「需要函数套函数，然后 return 一个函数的呀！」
```
function foo() {
  var local = 1;
  function bar() {
    local++;
    return local;
  }
  return bar; // return  window.bar = bar;
}
var func = foo();
func();
```
##### 为什么要函数套函数呢？
* 因为需要局部变量，所以才把 local 放在一个函数里，如果不把 local 放在一个函数里，local 就是一个全局变量了，达不到使用闭包的目的——隐藏变量
闭包的原文是 Closure，跟「包」没有任何关系。
所以函数套函数只是为了造出一个局部变量，跟闭包无关。

##### 为什么要 return bar 呢？
* 因为如果不 return，你就无法使用这个闭包。把 return bar 改成 window.bar = bar 也是一样的，只要让外面可以访问到这个 bar 函数就行了。
* 所以 return bar 只是为了 bar 能被使用，也跟闭包无关。

##### 闭包的作用
* 闭包常常用来「间接访问一个变量」。换句话说，「隐藏一个变量」
* 暴露一个访问器（函数），让别人可以「间接访问」

##### 你们闭包会造成内存泄漏？
* 内存泄露是指你用不到（访问不到）的变量，依然占居着内存空间，不能被再次利用起来
* 闭包里面的变量明明就是我们需要的变量，凭什么说是内存泄露？
    * 因为 IE。IE 有 bug，IE 在我们使用完闭包之后，依然回收不了闭包里面引用的变量
    * 这是 IE 的问题，不是闭包的问题。<a href="http://www.cnblogs.com/rubylouvre/p/3345294.html#undefined" style="color: blue;">参见司徒正美的这篇文章</a>

##### 立即执行函数
* 声明一个匿名函数
* 马上调用这个匿名函数
```
    (function(){alert('我是匿名函数')})()
```

##### 那么为什么还要用另一对括号把匿名函数包起来呢？
* 为了兼容 JS 的语法
* 不加的话，览器会报语法错误。想要通过浏览器的语法检查，必须加点小东西，比如下面几种都OK👌
```
    (function(){alert('我是匿名函数')} ()) // 用括号把整个表达式包起来
    (function(){alert('我是匿名函数')}) () //用括号把函数包起来
    !function(){alert('我是匿名函数')}() // 求反，我们不在意值是多少，只想通过语法检查
    +function(){alert('我是匿名函数')}()
    -function(){alert('我是匿名函数')}()
    ~function(){alert('我是匿名函数')}()
    void function(){alert('我是匿名函数')}()
    new function(){alert('我是匿名函数')}()
```

##### 立即执行函数有什么用？
* 创建一个独立的作用域。使这个作用域里面的变量，外面访问不到（即避免「变量污染」）
* 以下经典案列
```
var liList = ul.getElementsByTagName('li')
for(var i = 0; i < 6; i++) {
  liList[i].onclick = function() {
    alert(i); // 为什么 alert 出来的总是 6，而不是 0、1、2、3、4、5
  }
}
```
i 是贯穿整个作用域的，而不是给每个 li 分配了一个 i，用户一定是在for运行完之后才点击，此时都i为6
用立即执行函数给每个 li 创造一个独立作用域即可（当然还有其他办法: 如 let）：
<a href="https://jsbin.com/gicusajoqi/edit?html,js,output" style="color: blue;">还原效果，click me!</a>
```
var ul = document.getElementsByTagName('ul')[0]
var liList = ul.getElementsByTagName('li');
for(var i = 0; i < 6; i++) {
  !function(ii) {
    liList[ii].onclick = function() {
      alert(ii); // 0、1、2、3、4、5
    }
  }(i)
}
```
在立即执行函数执行的时候，i 的值被赋值给 ii，此后 ii 的值一直不变。
i 的值从 0 变化到 5，对应 6 个立即执行函数，这 6 个立即执行函数里面的 ii 「分别」是 0、1、2、3、4、5。
