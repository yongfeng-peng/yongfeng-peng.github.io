---
title: css常见功能代码
date: 2020-08-27 10:39:44
categories:
- CSS
tags: css常见的效果实现
---

##### CSS3 animation实现点点点loading动画
```
@supports (display:none) {
  dot {
    display: inline-block; 
    width: 3ch;
    text-indent: -1ch;
    vertical-align: bottom; 
    overflow: hidden;
    animation: dot 3s infinite step-start both;
    font-family: Consolas, Monaco, monospace;
  }
}

@keyframes dot {
  33% { text-indent: 0; }
  66% { text-indent: -2ch; }
}
<a href="javascript:" class="grebtn">订单提交中<dot>...</dot></a>
```

##### 按钮加一个类名自动变菊花loading状态无图片版
```
/* 按钮loading */
a[class*=-btn].loading, 
label[class*=-btn].loading {
  position: relative;
}
a[class*=-btn].loading::first-line, 
label[class*=-btn].loading::first-line {
  color: transparent;
}
a[class*=-btn].loading::before, 
label[class*=-btn].loading::before {
  width: 4px; height: 4px;
  margin: auto;
  content: '';
  -webkit-animation: spinZoom 1s steps(8) infinite;
  animation: spinZoom 1s steps(8) infinite;
  border-radius: 100%;
  box-shadow: 0 -10px 0 1px currentColor, 10px 0 currentColor, 0 10px currentColor, -10px 0 currentColor, -7px -7px 0 .5px currentColor, 7px -7px 0 1.5px currentColor, 7px 7px currentColor, -7px 7px currentColor;
  /* center */
  position: absolute;
  top: 0; right: 0; bottom: 0; left: 0;
}
/* loading动画 */
@-webkit-keyframes spinZoom {
  0% {
    -webkit-transform: scale(.75) rotate(0);
  }
  100% {
    -webkit-transform: scale(.75) rotate(360deg);
  }
}
@keyframes spinZoom {
  0% {
    transform: scale(.75) rotate(0);
  }
  100% {
    transform: scale(.75) rotate(360deg);
  }
}
<button><a class="a-btn loading"></a>3445</button>
```