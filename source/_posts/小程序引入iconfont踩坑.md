---
title: 小程序引入iconfont踩坑
date: 2019-07-03 16:48:26
tags:
---

#### 小程序引入iconfont踩坑
* 在<a href="https://www.iconfont.cn/" style="color: blue;">阿里巴巴</a>把需要的图标加入购物车
* 添加到项目，没项目新建一个
* 下载到本地
* 找到iconfont.css把代码复制到小程序项目到app.wxss或者拷贝到static,通过css方式引入**不想全局引用，可单独在对应wxss中使用**
```
    @import "./static/iconfont/iconfont.wxss"
```
* 替换iconfont.css中的@font-face为上面的生成代码
* 使用
```
    <text class='iconfont icon-jiazai'></text>
```
*  **注意是iconfont不是icon** 坑了我几个小时，还是踩坑太少
* 每次重新添加icon到项目需要**重新替换**@font-face为上面的生成代码，并且把新添加的图标加入iconfont.wxss
```
@font-face {
  font-family: 'iconfont';  /* project id 1274544 */
  src: url('//at.alicdn.com/t/font_1274544_5eej6szwu1j.eot');
  src: url('//at.alicdn.com/t/font_1274544_5eej6szwu1j.eot?#iefix') format('embedded-opentype'),
  url('//at.alicdn.com/t/font_1274544_5eej6szwu1j.woff2') format('woff2'),
  url('//at.alicdn.com/t/font_1274544_5eej6szwu1j.woff') format('woff'),
  url('//at.alicdn.com/t/font_1274544_5eej6szwu1j.ttf') format('truetype'),
  url('//at.alicdn.com/t/font_1274544_5eej6szwu1j.svg#iconfont') format('svg');
}
```
* 看起来很简单，可自己就是犯了很多低级错误rve