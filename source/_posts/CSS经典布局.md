---
title: CSS经典布局
date: 2020-08-26 14:39:21
categories:
- CSS
tags: 一行代码实现（主要基于flex、grid布局）
---


#### CSS经典布局
##### 空间居中对齐
* 空间居中布局指的是，不管容器的大小，项目总是占据中心点。
```
.container {
  display: grid;
  place-items: center/end/start;
}

/* place-items: <align-items> <justify-items>; */

```

##### 并列式布局
* 并列式布局就是多个项目并列。如果宽度不够，放不下的项目就自动折行。
```
.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.item{
  flex: 0 1 150px;
  margin: 5px;
}

```
* flex属性是flex-grow、flex-shrink、flex-basis这三个属性的简写形式。
* flex: <flex-grow> <flex-shrink> <flex-basis>;
  * flex-basis：项目的初始宽度。
  * flex-grow：指定如果有多余宽度，项目是否可以扩大。
  * flex-shrink：指定如果宽度不足，项目是否可以缩小。
  * flex: 0 1 150px;的意思就是，项目的初始宽度是150px，且不可以扩大，但是当容器宽度不足150px时，项目可以缩小。
* 如果写成flex: 1 1 150px;，就表示项目始终会占满所有宽度

##### 两栏式布局
* 两栏式布局就是一个边栏，一个主栏。
* 边栏始终存在，主栏根据设备宽度，变宽或者变窄。如果希望主栏自动换到下一行，可以参考上面的"并列式布局"。
```
.container {
  display: grid;
  grid-template-columns: minmax(150px, 25%) 1fr;
}
/* grid-template-columns指定页面分成两列。第一列的宽度是minmax(150px, 25%)，即最小宽度为150px，最大宽度为总宽度的25%；第二列为1fr，即所有剩余宽度。 */
```

##### 三明治布局
* 三明治布局指的是，页面在垂直方向上，分成三部分：页眉、内容区、页脚。
* 这个布局会根据设备宽度，自动适应，并且不管内容区有多少内容，页脚始终在容器底部（粘性页脚）。也就是说，这个布局总是会占满整个页面高度。
```
.container {
  display: grid;
  grid-template-rows: auto 1fr auto;
}
/* 指定采用 Grid 布局。核心代码是grid-template-rows那一行，指定垂直高度怎么划分，这里是从上到下分成三部分。第一部分（页眉）和第三部分（页脚）的高度都为auto，即本来的内容高度；第二部分（内容区）的高度为1fr，即剩余的所有高度，这可以保证页脚始终在容器的底部。 */
```
