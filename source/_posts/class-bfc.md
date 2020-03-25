---
title: BFC
date: 2019-05-02 10:58:55
tags:
---

#### BFC
* <a href="https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context">详细概念 MDN</a>
* BFC全称为Block Formatting Context,即块级格式化上下文，就是**一个独立的渲染区域**
* 可以简单理解为： BFC为**某个元素的一个CSS属性**，只不过这个属性不能**被开发者显式的修改**，拥有这个属性的元素对内部元素和外部元素会表现出一些特性，这就是BFC。

#### BFC特点：
* BFC这个元素垂直方向的边距会发生重叠
* BFC的区域不会与浮动元素的box重叠
* BFC在页面上是一个独立的元素，内外元素不会互相影响
* BFC计算高度的时候，浮动元素也会计算

#### 如何创建BFC
* float: 值不为none
* overflow: 值为hidden或atuo给父元素
* position：值不为static或relative
* display：值为table(table、table-cell、table-caption等)相关的属性

#### BFC应用场景
* 了解BFC主要知道它的应用场景就可以了
1.防止垂直margin重叠：
    * 父子元素的边界重叠解决方案：在父元素上加上overflow:hidden;使其成为BFC
```
 <style>
    .parent {
        background: #E7A1C5;
        overflow: hidden;
    }
    .parent .child {
        background: #C8CDF5;
        height: 100px;
        margin-top: 10px;
    }
</style>
<section class="parent">
    <article class="child"></article>
</section>
```

2. 兄弟元素的边界重叠，在第二个子元素创建一个BFC上下文
```
<style>
    #margin {
        background: #E7A1C5;
        overflow: hidden;
        width: 300px;
    }
    #margin  p {
        background: #C8CDF5;
        margin: 20px auto 30px;
    }
</style>
<section id="margin">
    <p>1</p>
    <div style="overflow:hidden;">
        <p>2</p>
    </div>
    <p>3</p>
</section>
```

3. 自适应两栏布局 
* BFC不会与float元素发生重叠
```
<section id="layout">
    <style>
        #layout {
            background: red;
        }
        #layout .left {
            float: left;
            width: 100px;
            height: 100px;
            background: pink;
        }
        #layout .right {
            height: 110px;
            background: #ccc;
            overflow: auto;
        }
    </style>
    <div class="left"></div>
    <div class="right"></div>
</section>
```

4. BFC清除内部浮动
* 父元素#float的高度为0，解决方案为为父元素#float创建BFC，这样浮动子元素的高度也会参与到父元素的高度计算
```
 <section id="float">
    <style>
        #float {
            overflow: auto;
            background: red;
        }
        #float .child {
            float: left;
            font-size: 30px;
        }
    </style>
    <div class="child">我是浮动元素</div>
</section>
```
