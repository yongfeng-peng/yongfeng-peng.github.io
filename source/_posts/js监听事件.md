---
title: js监听事件
date: 2020-04-24 10:19:02
categories:
- JS
tags:
---

#### js监听事件的叠加和移除
##### 监听事件
* html DOM元素有很多on开头的监听事件，如onload、onclick等，<a href="https://www.runoob.com/jsref/dom-obj-event.html">见DOM事件列表</a>。但是**同一种事件**，后面注册的**会覆盖前面**的
```
window.onresize = () => {
    alert(1);
}
window.onresize = () => {
    alert(2);
}
// 改变窗口大小时，只会弹出2
```
##### addEventListener监听
* element.addEventListener(type，handler，false/true)
  * type: 事件类型
  * handler: 事件执行触发的函数
  * false/true: false为冒泡/ture为捕获，参数是true，表示在捕获阶段调用事件处理程序；参数是false，表示在冒泡阶段调用事件处理程序。
  * 事件捕获：父级元素先触发，子集元素后触发；
  * 事件冒泡：子集元素先触发，父级元素后触发；
  * 一般的绑定事件，都是采用冒泡方式，即false

* 利用addEventListener添加监听事件，可以**重复添加**，但是并不会**互相覆盖**
```
window.addEventListener('resize', () => {
    alert(1);
})
window.addEventListener('resize', () => {
    alert(2);
})
// 改变窗口大小时，先后弹出1和2
```
##### removeEventListener移除监听
* removeEventListener跟addEventListener相对应，用于移除事件监听
* 如果要移除事件句柄，addEventListener() 的执行函数必须使用**外部具名函数**，**匿名函数事件是无法移除哒**
```
// 匿名函数事件无法移除
window.addEventListener('resize', () => {
    alert(1);
})
 
// 监听具名函数事件
function myResize() {
    alert(2);
}
window.addEventListener('resize', myResize);

// 移除事件监听
window.removeEventListener('resize', myResize);
```
##### dispatchEvent() 允许发送事件到监听器上
* 标准浏览器中使用dispatchEvent派发自定义事件：element.dispatchEvent()，除此之外，还有创建和初始化事件：
* 一般的流程是：创建 >> 初始化 >> 派发。
* 对应的事件流程：document.createEvent() >> event.initEvent() >> element.dispatchEvent()
  * createEvent()方法返回新创建的Event对象，支持一个参数，表示事件类型
  * initEvent()方法用于初始化通过DocumentEvent接口创建的Event的值。 支持三个参数：initEvent(eventName, canBubble, preventDefault). 分别表示： 事件名称，是否可以冒泡，是否阻止事件的默认操作
  * dispatchEvent()就是触发执行了，element.dispatchEvent(eventObject), 参数eventObject表示事件对象，是createEvent()方法返回的创建的Event对象
```
<!-- let el = document.getElementById("eventId");
el.addEventListener('customEvent', () => {
  alert('弹弹弹，弹走鱼尾纹~~');
})
// 创建
let evt = document.createEvent('eventName');
// 初始化
evt.initEvent('alert', false, false);
// 触发
dom.dispatchEvent(evt); -->

// document上绑定自定义事件oneating  
document.addEventListener('customEvent', (event) =>  {  
  alert(`${event.mingzi}, ${event.message}`);
}, false);  
// 创建event的对象实例。  
var event = document.createEvent('HTMLEvents');  
// 3个参数：事件类型，是否冒泡，是否阻止浏览器的默认行为  
event.initEvent('customEvent', true, true);  
/*属性，随便自己定义*/  
event.mingzi = 'hello,我是xx';  
event.message = '我今天18岁';  
// 触发自定义事件oneating  
document.dispatchEvent(event);
```


