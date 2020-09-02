---
title: HTML、CSS基础概念总结
date: 2019-05-04 16:37:46
categories:
- CSS
tags:
---
#### HTML
##### HTML语义化
* HTML 语义化就是使用正**确的标签**，内容的结构化（内容语义化），选择合适的标签（代码语义化）
* 定义麻烦，可以直接**举例**，如：
    * 段落就写 p 标签，标题就写 h1~h6 标签，
    * 文章就写article标签，视频就写video标签，section就写文档中的一个区域（或节），article比section更具有独立性、完整性。可通过该段内容脱离了所在的语境，是否完整、独立来判断等等。
* 也可以**阐述**一下历史
    * 咱们的后台开发人员常使用table布局，美工人员常使用div+css布局，前端的优势当然就是会使用正确的标签进行页面开发啦

##### 语义话的好处
*  可简单解释为：便于**开发者**阅读和写出更优雅的代码的同时让**浏览器**的爬虫和机器很好地解析
1. 在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构-----为了裸奔时好看
2. 有利于<a href ="https://baike.baidu.com/item/%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E4%BC%98%E5%8C%96/3132?fromtitle=SEO&fromid=102990" style="color: blue;">**SEO**</a>：和搜索引擎建立良好沟通，有助于<a href="https://baike.baidu.com/item/%E7%BD%91%E7%BB%9C%E7%88%AC%E8%99%AB/5162711?fromtitle=%E7%88%AC%E8%99%AB&fromid=22046949" style="color: blue;">**爬虫**</a>抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重
3. 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以有意义的方式来渲染网页
4. 有利于团队开发和维护

##### meta viewport是做什么呢？有什么用呢？
* 先了解一下不设置meta viewport标签会导致什么后果呢
    * 移动设备上浏览器默认的宽度值为800px，980px，1024px等，总之是大于屏幕宽度的。这里的宽度所用的单位px都是指css中的px，它与实际屏幕物理像素的px不是一回事
* meta标签的就是让**当前viewport的宽度等于设备的宽度**，同时不允许用户手动缩放
    * 因为每个移动设备浏览器中都有一个理想的宽度，可以用meta标签把viewport的宽度设为那个理想的宽度
```
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1">
```
* width: 设置layout viewport的宽度，为一个正整数，或字符串"width-device"
* height: 设置layout viewport的高度，这个属性对我们并不重要，很少使用
* initial-scale: 设置页面的初始缩放值，为一个数字，可以带小数
* minimum-scale: 允许用户的最小缩放值，为一个数字，可以带小数
* maximum-scale: 允许用户的最大缩放值，为一个数字，可以带小数
* user-scalable	是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许

想深入了解的孩童可以看看：<a href="https://www.cnblogs.com/2050/p/3877280.html" style="color: blue;">click here！</a>

##### HTML 5 标签都有
1. <section> : 定义文档中的一个章节
2. <nav> : 定义只包含导航链接的章节
3. <article> : 定义可以独立于内容其余部分的完整独立内容块
4. <aside> : 定义和页面内容关联度较低的内容——如果被删除，剩下的内容仍然很合理
5. <header> : 定义页面或章节的头部。它经常包含 logo、页面标题和导航性的目录
6. <footer> : 定义页面或章节的尾部。它经常包含版权信息、法律信息链接和反馈建议用的地址
7. <address>: 定义包含联系信息的一个章节
8. <main>: 定义文档中主要或重要的内容
<a href="https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list" style="color: blue;">想了解更多:)</a>

##### H5 理解
* 简单理解：H5表示移动端页面，反正不是HTML5
* H5更应该被理解为针对酷炫的移动端页面的一种**解决方案**，而这个解决方案里包含HTML5新增的标签(audio/video),canvas技术，拖拽的特性，本地存储，websocket通信，也会用到基本的盒模型，定位等一些**前端基本知识**，所以，H5更应该理解为一个技术合集，前端的技术广度/深度
* H5移动端各种产品的简称吧

#### css
##### 两种盒模型分别说一下
* css盒模型的包括（margin + padding + content）
* 标准模型(content) + IE模型(content+padding+border)
* 两者的区别：计算宽度和高度的不同

##### css如何设置这两种模型
* (标准)box-sizing: content-box         
* (IE)box-sizing: border-box

##### js如何设置获取盒模型的宽和高
1. dom.style.width/height(获取的是内联)
2. dom.currentStyle.width/height(获取页面渲染之后的，只支持IE)
3. window.getComputedStyle(dom).width/height(firefox google)
4. dom.getBoundingClientRect().width/height(计算元素的绝对位置，获取的是viewport左上角的位置)
平时开发**更多使用border-box**，因为更好用

##### CSS 选择器优先级
* 普通的权重比较，**不是完全正确** 
    * （外部样式）External style sheet <（内部样式）Internal style sheet <（内联样式）Inline style 
    1.  内联样式表的权值最高 1000
    2.  ID 选择器的权值为 100
    3.  Class 类选择器的权值为 10
    4.  HTML 标签选择器的权值为 1
    * <a href="https://www.cnblogs.com/xugang/archive/2010/09/24/1833760.html" style="color: blue;">详细参考</a>

* 越具体优先级越高
* 优先级写在后面的覆盖写在前面的
* !important 优先级最高，但是要**少用**

##### 清除浮动
1. 父级div定义伪类：after **强烈推荐**
```
.clearfix:after {
    content: '';
    display: block; /* 或者table */
    clear: both;
}
.clearfix{
    zoom: 1; /* IE 兼容 */
}
```
2. 在结尾处添加空div标签clear: both
3. 父级div定义height
4. 父级div定义overflow: hidden
5. 父级div定义overflow: auto

##### flex布局左边一栏 右边两栏
**margin-left: auto**
```
 <style>
    .box {
        display: flex;
        border: 1px solid red;
        justify-content: space-between;
        padding: 10px 0;
    }
    .box div {
        width: 300px;
        height: 100px;
        border: 1px solid #ff9400;
    }
    .box div:nth-child(2) {
        margin-left: auto;
    }
</style>
<div class="box">
    <div>1</div>
    <div>2</div>
    <div>3</div>
</div>
```