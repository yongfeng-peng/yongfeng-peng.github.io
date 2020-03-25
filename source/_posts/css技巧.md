---
title: css技巧
date: 2019-07-10 09:43:41
categories:
- css技巧总结
tags: 
- css
---

#### css的一些使用技巧（含css3）
##### 按钮渐变动画效果
* 改变渐变的位置
```
button {
    background-image: linear-gradient(#5187c4, #1c2f45);
    background-size: auto 200%;
    background-position: 0 100%;
    transition: background-position 0.5s;
}    
button:hover {
    background-position: 0 0;
}
```
##### 包裹长文本（不确定长度）
```
pre {
    white-space: pre-line;
    word-wrap: break-word;
}
```
##### css设置模糊文本
```
.blurry-text {
   color: transparent;
   text-shadow: 0 0 5px rgba(0,0,0,0.5);
}
```
##### 用css实现省略号动画
```
.loading:after {
    overflow: hidden;
    display: inline-block;
    vertical-align: bottom;
    content:  "\2026"; /* ascii code for the ellipsis character */
    animation: ellipsis 2s infinite;
}
@keyframes ellipsis {
    from {
        width: 2px;
    }
    to {
        width: 15px;
    }
}
```
##### 样式重置
```
html, body, div, span, applet, object, iframe, h1, h2, h3, h4, h5, h6, p, blockquote, pre, a, abbr, acronym, address, big, cite, code, del, dfn, em, img, ins, kbd, q, s, samp, small, strike, strong, sub, sup, tt, var, b, u, i, center, dl, dt, dd, ol, ul, li, fieldset, form, label, legend, table, caption, tbody, tfoot, thead, tr, th, td, article, aside, canvas, details, embed, figure, figcaption, footer, header, hgroup, menu, nav, output, ruby, section, summary, time, mark, audio, video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
    outline: none;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}
html { height: 101%; }
body { font-size: 62.5%; line-height: 1; font-family: Arial, Tahoma, sans-serif; }
article, aside, details, figcaption, figure, footer, header, hgroup, menu, nav, section { display: block; }
ol, ul { list-style: none; }
blockquote, q { quotes: none; }
blockquote:before, blockquote:after, q:before, q:after { content: ''; content: none; }
strong { font-weight: bold; } 
table { border-collapse: collapse; border-spacing: 0; }
img { border: 0; max-width: 100%; }
p { font-size: 1.2em; line-height: 1.0em; color: #333; }
```
##### css清除浮动
```
.clearfix:after {
    content: "."; 
    display: block;
    clear: both;
    visibility: hidden;
    line-height: 0;
    height: 0; 
}
.clearfix { display: inline-block; }
html[xmlns] .clearfix { display: block; }
* html .clearfix { height: 1%; }
```
```
.clearfix:before, .container:after { content: ""; display: table; }
.clearfix:after { clear: both; }
/* IE 6/7 */
.clearfix { zoom: 1; }
```
##### 自定义文本选中后的颜色
```
.txt::selection { 
    background: #e2eae2; 
}
```
##### css设置背景渐变
```
.colorbox {
    background: #629721;
    background-image: -webkit-gradient(linear, left top, left bottom, from(#83b842), to(#629721));
    background-image: -webkit-linear-gradient(top, #83b842, #629721);
    background-image: -moz-linear-gradient(top, #83b842, #629721);
    background-image: -ms-linear-gradient(top, #83b842, #629721);
    background-image: -o-linear-gradient(top, #83b842, #629721);
    background-image: linear-gradient(top, #83b842, #629721);
}
```
##### css原图案
```
body {
    background: radial-gradient(circle, white 10%, transparent 10%),
    radial-gradient(circle, white 10%, black 10%) 50px 50px;
    background-size: 100px 100px;
}
```
* <a href="https://juejin.im/post/5b1f41246fb9a01e725131fb">To Learn More...</a>

##### flex-wrap
* 遇到问题自动换行木有效果
**子元素需要设置具体的宽度**
```
flex-wrap: wrap; // 父元素设置
flex: 1; // 子元素不能这样设置，必须是具体的宽度
```
##### emmet语法
* ul>li*3>{hello$}

##### css实现边框齿轮效果
* <a href="http://www.fly63.com/article/detial/521">To Learn More...</a>
```
<style>
    .toothbg {
        width: 100%;
        height: 20px;
        background: #ff94000;
        background-image:-webkit-gradient(linear,50% 0,0 100%,from(transparent), color-stop(.5,transparent),color-stop(.5,#e5e5e5),to(#e5e5e5)),
                        -webkit-gradient(linear,50% 0,100% 100%,from(transparent), color-stop(.5,transparent),color-stop(.5,#e5e5e5),to(#e5e5e5));
        background-image:-moz-linear-gradient(50% 0 -45deg,transparent,transparent 50%,#e5e5e5 50%,#e5e5e5),
                        -moz-linear-gradient(50% 0 -135deg,transparent,transparent 50%,#e5e5e5 50%,#e5e5e5);                                
        background-size:30px 15px;
        background-repeat:repeat-x;
        background-position:0 100%;                    
    }
</style>
<div class="toothbg"></div>
```

##### css实现动画效果
* 国外网站参考
* <a href="https://reeoo.com/">To Learn More...</a>

##### css边框
```
border-style: none | solid | dashed | dotted | double;
```

##### css文本控制
* first-letter 选中首个字符(针对块元素有作用)
```
.txt {
    margin: 100px auto;
    width: 100px;
    height: 100px;
    border: 1px double #ff9400;
    text-align: center;
}
.txt::first-letter {
    font-size: 40px;
    color: #ff0000;
}

<p class="txt">¥ 200</p>
```
* text-transform 指定文本大小写(应用在假设输入框只能输入大小写)
* capitalize 首字母大写 | uppercase 全部大写 lowercase 全部小写 none
```
transform: capitalize | uppercase | lowercase | none;
```
* word-spacing 指定空格间隙（前后空格没有用，设置的是**字符**之间空格）
```
word-spacing: 10px;
```
* white-space 空白符处理
* <a href="https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space" style="color: blue;">To Learn More...</a>
```
white-space: normal | nowrap | pre | pre-wrap | pre-line;
```
* text-align: justify 设置两端对齐
```
<style>
div {
  outline: 1px solid;
}
span {
  display: inline-block;
  width: 100px;
  line-height: 100px;
  border-bottom: 1px solid lightcyan;
  text-align: center;
  background: cyan;
}
i {
  display: inline-block;
  width: 100px;
  outline: 1px dashed;
}
</style>
<div>
    <span>1</span>
    <span>2</span>
    <span>3</span>
    <span>4</span>
    <span>5</span>
    <i></i><i></i><i></i>x
</div>
```