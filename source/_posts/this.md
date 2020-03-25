---
title: this
date: 2019-05-05 15:31:36
tags:
---

#### this到底指向谁呢？
<a href="https://zhuanlan.zhihu.com/p/25991271" style="color: blue;">惊喜在这里</a>
##### 看自己平时积累了
1. fn()           this => window/global
2. obj.fn()       this => obj
3. fn.call(xx)    this => xx
4. fn.apply(xx)   this => xx
5. fn.bind(xx)    this => xx
6. new Fn()       this => 新创建的对象
7. fn = ()=> {}   this => 外面的 this

##### 看调用
* 函数调用
* JS（ES5）里面有三种函数调用形式
    * func(p1, p2) 
    * obj.child.method(p1, p2)
    * func.call(context, p1, p2)
* 一般我们都比较倾向于使用前两种，第三种不知道什么鬼东西呀
其他两种都是语法糖，可以等价地变为 call 形式：
```
func(p1, p2) 等价于 func.call(undefined, p1, p2)
obj.child.method(p1, p2) 等价于 obj.child.method.call(obj.child, p1, p2)
```
**函数调用只有一种形式** ： func.call(context, p1, p2), context就是this

##### 看看下面的举例
```
function func() {
  console.log(this)
}
func();
// 等价转化一下
func.call(undefined) // 可简写为 func.call()， 结果就是window
```
是不是疑问应该是undefined呢？可不能忘记**浏览器**里有一条规则：
* 如果你传的context是 null 或者 undefined，那window 对象就是默认的 context（但严格模式下（'use strict'）默认 context 是 undefined）

如果你不想this是window，好说，那就这样吧： func.call(obj) // 那么里面的 this 就是 obj 对象了
```
var obj = {
  foo: function() {
    console.log(this);
  }
}
obj.foo(); // 等价转化为obj.foo.call(obj) ，结果就是obj
```

看看下面的代码，是不是so easy! :(
```
var obj = {
  foo: function() {
    console.log(this);
  }
}
var bar = obj.foo;
obj.foo(); // 转换为 obj.foo.call(obj)，this 就是 obj
bar() 
// 转换为 bar.call(), 没有传 context, 所以 this 就是 undefined
// 最后浏览器给你一个默认的 this —— window 对象
```
##### 类似 [] 的，this怎么指向呢
```
function fn () { 
    console.log(this);
}
var arr = [fn, fn2];
arr[0]() // arr 转换为 arr.0() arr.0.call(arr) (语法是有问题的，这样推理木有问题呀)
```
##### 总结
* **this 就是你 call 一个函数时，传入的第一个参数**
* 如果你的函数调用形式不是 call 形式，请**[转换代码]**将其转换为 call 形式。

##### 问题又来了，Event Handler 中的 this 指什么呢？
不知道ddEventListener 的源代码，怎么办呢？
```
btn.addEventListener('click' ,function handler(){
  console.log(this) // 请问这里的 this 是什么
})
```
<a href="https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener#The_value_of_this_within_the_handler" style="color: blue;">MDN 对this解释</a>
* 通常来说this的值是触发事件的元素的引用，这种特性在多个相似的元素使用同一个通用事件监听器时非常让人满意。 
* 当使用 addEventListener() 为一个元素注册事件的时候，句柄里的 this 值是该元素的引用。其与传递给句柄的 event 参数的 currentTarget 属性的值一样。
所以你可以想浏览器的源码是这样写的：
```
// 当事件被触发时
handler.call(event.currentTarget, event) 
// 那么 this 是什么不言而喻
```
##### jQuery Event Handler 中的 this
下面代码中的 this 是什么呢：
```
$ul.on('click', 'li' , function(){
  console.log(this)
})
```
<a href="https://www.jquery123.com/on/" style="color: blue;">jQuery 文档是这样写的:</a>
* 当jQuery的调用处理程序时，this关键字指向的是当前正在执行事件的元素。对于直接事件而言，this 代表绑定事件的元素。
* **对于代理事件而言，this 则代表了与 selector 相匹配的元素**。(注意，如果事件是从后代元素冒泡上来的话，那么 this 就有可能不等于 event.target。)若要使用 jQuery 的相关方法，可以根据当前元素创建一个 jQuery 对象，即使用 $(this)。

##### 总结一下如何确定 this 的值
1. 看源码中对应的函数是怎么被 call 的（这是最靠谱的办法）
2. 看文档
3. console.log(this)
4. 不要瞎猜，你猜不到的:(

##### 如何强制指定 this 的值，call/apply 即可：
```
function handlerWrapper(event) {
  function handler() {
    console.log(this); // 请问这里的 this 是什么
  }
  handler.call({name: 'xlc'}, event)
}
btn.addEventListener('click', handlerWrapper);
// 也可以直接使用bind
function handler() {
  console.log(this); // 请问这里的 this 是什么
}
var handlerWrapper = handler.bind({name: 'xlc'})
btn.addEventListener('click', handlerWrapper);
// 上面代码可以简化为
btn.addEventListener('click', function() {
  console.log(this); // 请问这里的 this 是什么
}.bind({name: 'xlc'}))
```



