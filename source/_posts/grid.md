---
title: grid
date: 2019-07-09 18:20:50
tags:
---

#### grid 布局优势
* 固定或弹性的轨道尺寸
* 定位项目
* 创建额外的轨道来保存内容
* 对齐控制
* 控制重叠内容（z-index）

#### grid VS flexbox
* flexbox是yi w一维布局，只能在一条直线上放置内容区块
* grid是二维布局，根据设计需求将内容区块放置到任何地方

* 实际上
    * grid和flexbox可以很好到
* Grid Container网格容器
    * 元素应用display: grid,他是其所有网格项的父元素
```
.container {
    display: grid;
}
<div class="container">
    <div class="item">One</div>
    <div class="item">Two</div>
</div>
```
* Grid Item 网格项
    * 网格容器的子元素，下面的item元素是网格项
* Grid Line 网格线 
    * 组成网格项的分界线
* Grid Track 网格轨道
    * 两个相邻的网格线之间的网格轨道
* Grid Cell 网格单元
    *两个相邻的列网格线和两个相邻的行网格线组成的是网格单元
* Grid Area 网格区域
    * 四个网格线包围的总空间

* 单位
    * fr(单位)
    * 剩余空间分配数，用于在一系列长度值中分配剩余空间，如果多个已指定了多个部分，则剩余的空间根据各自的数字按比例分配
    * gr(单位)-----暂未认证
    * 网格数
* display: grid|inline-grid|subgrid
* 注意
    * 当元素设置了网格布局,column, float, clear, vertical-align属性无效
    * subgrid目前所有浏览器不支持
* grid-template
* grid-template-columns 列
* grid-template-rows 行推荐值，使用auto（高度确定平均分布，高度不确定平均收缩）
    * 属性值
        * 轨道大小(track-size)：可以使用css长度(px,em等)，百分比或用分数（用fr单位）
        * 网格线名字(line-name)：可以选择任用名字
* grid-template-area:网格区域的名称来定义网格区域
    * none 
    * . empty
```
grid-template-areas: "header header header header"
                    "main main . slider"
                    "footer footer . footer"
```
* grid-column-gap/grid-row-gap 设置网格间距
* 设置行列简写
    * grid-gap: grid-row-gap行 grid-column-gap列;
    * gap: grid-row-gap行 grid-column-gap列;
```
grid-column-gap: 10px;
grid-row-gap: 15px;
```
* items
* justify-items: start | end | center | stretch（内容宽度占据整个网格区域）; // 沿着行轴(水平)对齐网格内的内容
* align-items

##### （grid布局应用）一行css代码搞定响应式
* fraction单位值，很容易更改列的宽度
```
<style>
 .container {
    display: grid;
    /* grid-template-columns: 100px 100px 100px; */
    grid-template-columns: 1fr 2fr 1fr;
    grid-template-rows: 50px 50px;
}
</style>
<div class="container">
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
    <div>5</div>
    <div>6</div>
</div>
```
* 上面列子总是三列宽，预期希望网格能根据容器的宽度改变列的数量
* repeat(param1, param2)函数，强大的指定列和行的方法，param1指定行与列的数量，param2指定行与列的宽度
```
 .container {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(2, 50px);
    border: 1px solid #ddd;
}
```
* auto-fit 可跳过固定数量的列，将3替换为自适应数量
```
 .container {
    display: grid;
    grid-gap: 5px; // 设置网格之间的间距
    grid-template-columns: repeat(auto-fit), 100px);
    grid-template-rows: repeat(2, 50px);
    border: 1px solid #ddd;
}
```
* auto-fit 自适应设置，存在问题，网格右侧通常留有空白部分
    **解决方法minmax()**
    * minmax()函数定义的范围大于或等于 min， 小于或等于 max。
```
.container {
    display: grid;
    grid-gap: 5px;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-template-rows: repeat(2, 100px);
}
``` 
* 为网格添加图片，使图片适应于每个条目，将其宽高设置为于条目本身一样
    **使用object-fit: conver**
    * 将使图片覆盖它的整个容器，根据需要，浏览器将会对其进行裁剪
```
<style>
 .container {
    display: grid;
    grid-gap: 5px;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-template-rows: repeat(2, 200px);
}
div {
    border: 1px solid #ff9400;
}
.container > div > img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
</style>
<div class="container">
    <div>
        <img src="./self.jpg" alt="">
    </div>
    <div>
        <img src="./self.jpg" alt="">
    </div>
    <div>
        <img src="./self.jpg" alt="">
    </div>
    <div>
        <img src="./self.jpg" alt="">
    </div>
    <div>
        <img src="./self.jpg" alt="">
    </div>
    <div>
        <img src="./self.jpg" alt="">
    </div>
</div>
```