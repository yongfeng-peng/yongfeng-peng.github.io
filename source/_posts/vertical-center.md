---
title: 实现垂直居中、flex
date: 2019-05-04 21:49:01
tags:
---

#### 实现垂直居中
* 如果父元素.parent的height未知，只需要**padding: 10px 0**; 就能将子元素.child垂直居中
* 如果父元素.parent的height已知，就很难把子元素.child居中

##### 以下是垂直居中的方法
1. table布局实现
```
.parent {
  height: 500px;
  border: 1px solid #ff9400;
}

.child {
  width: 300px;
  border: 1px solid #ff0000;
}
<table class="parent">
    <tr>
        <td class="child">居中元素</td>
    </tr>
</table>
```
2. 给父元素添加伪元素（100% 高度的 afrer before 加上 inline block）
```
.parent {
  height: 500px;
  border: 1px solid #ff9400;
  text-align: center;
}

.child {
  width: 300px;
  display: inline-block;
  vertical-align: middle;
  border: 1px solid #ff0000;
}

.parent:after,
.parent:before {
  content: '';
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}
 <div class="parent">
    <div class="child">垂直居中元素</div>
</div>
```
3. table属性应用
```
.parent {
  display: table;
  height: 500px;
  border: 1px solid #ff9400;
  text-align: center;
}

.parent > div {
  width: 300px;
  display: table-cell;
  vertical-align: middle;
}

.child {
  border: 1px solid #ff0000;
}
<div class="parent">
    <div>
      <div class="child">垂直居中元素</div>
    </div>
</div>

```
4. position定位实现，margin设置负值
```
.parent {
  height: 500px;
  position: relative;
  border: 1px solid #ff9400;
}

.child {
  width: 300px;
  height: 100px;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -150px;
  margin-top: -50px;
  border: 1px solid #ff0000;
}

<div class="parent">
    <div class="child">垂直居中元素</div>
</div>
```
5. 也是定位，只是应用了css3 的 translate
```
.child {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  border: 1px solid #ff0000;
}
```
6. 同样是定位，不过巧妙的用了margin: auto
```
.child {
  width: 300px;
  height: 50px;
  position: absolute;
  margin: auto;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  border: 1px solid #ff0000;
}
```
7. 是不是终于到大家常用的flex啦
```
.parent {
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid #ff9400;
}

.child {
  width: 300px;
  border: 1px solid #ff0000;
}
```
想快速看到效果：<a href="https://jsbin.com/fayekiwita/edit?html,css,output" style="color: blue;">自己动手吧:)</a>
**能不写 height 就千万别写 height吧**
    欢迎👏贡献更多方法

#### flex
* flex的属性还是需要多实践操作，才会知道具体效果
* 不妨看看阮一峰老师的教程
<a href="http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html" style="color: blue;">flex-语法</a>
 <a href="http://www.ruanyifeng.com/blog/2015/07/flex-examples.html" style="color: blue;">flex-实例</a>


