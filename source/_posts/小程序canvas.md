---
title: 小程序canvas
date: 2019-07-13 11:10:24
tags:
---

#### 在小程序某一个页面使用canvas绘制多个动态圆环
* 封装的公共的canvas.js
```
// 绘制圆环动画共有方法
export default{ 
  // data: {
  //   percentage: '', //百分比
  //   animTime: '', // 动画执行时间
  // },
  options:{
    /*  @description {绘制圆形进度条方法}
      * @date 2019-7-13
      * @author pengyf
      * @param c: 绘制动画进度, id: canvas画板id, w,h: 圆心的横纵坐标
      * @returns 无
    */
    run(c, w, h, id) {
      // 需要在这里重新获取一下id，不然只能绘制一次
      let context = wx.createCanvasContext(id);
      let num = (2 * Math.PI / 100 * c) - 0.5 * Math.PI;
      context.arc(w, h, w - 8, -0.5 * Math.PI, num); // 每个间隔绘制的弧度
      // 设置为渐变的颜色
      var gradient = context.createLinearGradient(200, 100, 100, 200);
      gradient.addColorStop('0', '#2661DD');
      gradient.addColorStop('0.5', '#40ED94');
      gradient.addColorStop('1.0', '#5956CC');
      context.setStrokeStyle(gradient);
      // 设置单一的颜色
      // context.setStrokeStyle('#5ba2b6');
      context.setLineWidth('16');
      context.setLineCap('butt');
      context.stroke();
      context.beginPath();
      context.setFontSize(20); // 注意不要加引号
      context.setFillStyle('#ffef00');
      context.setTextAlign('center');
      context.setTextBaseline('middle');
      context.fillText(c + '%', w, h);
      context.draw();
    },

    /*  @description {绘制圆环的动画效果实现}}
      * @date 2019-7-13
      * @author pengyf
      * @param start: 起始百分比, end: 结束百分比, 
               w,h: 圆心的横纵坐标, id: canvas画板id
      * @returns 无
    */
    canvasTap(start, end, time, w, h, id) {
      start++;
      if (start > end) {
        return false;
      }
      this.run(start, w, h, id);
      setTimeout(() => {
        this.canvasTap(start, end, time, w, h, id);
      }, time);
    },

    /*  @description {绘制圆环}}
      * @date 2019-7-13
      * @author pengyf
      * @param id: canvas画板id, percent: 进度条百分比,time: 画图动画执行时间
      * @returns 无
    */
    draw(id, percent, animTime) {
      let time = animTime / percent;
      wx.createSelectorQuery().select('#' + id).boundingClientRect((rect) => {
        //监听canvas的宽高
        let w = parseInt(rect.width / 2); // 获取canvas宽的的一半
        let h = parseInt(rect.height / 2); // 获取canvas高的一半，
        this.canvasTap(0, percent, time, w, h, id);
      }).exec();
    }
  } 
}
```
* 在page页面使用
**使用了grid布局**
* wxml
```
<view class="contain">
  <view class="data">
    <view class="row" wx:for="{{canvasList}}" wx:key="{{index}}">
      <view class='bigCircle pos-center'></view>
      <view class='littleCircle pos-center'></view>
      <canvas canvas-id="{{item.id}}" id="{{item.id}}" class='canvas pos-center'></canvas>
    </view>
  </view>
</view>
```

* wxss
```
.data {
  width: 100%;
  /* display: flex;
  flex-wrap: wrap; */
  display: grid;
  grid-gap: 5px;
  grid-template-columns: repeat(auto-fit, minmax(350rpx, 1fr));
  grid-template-rows: repeat(2, 350rpx);
}
.data .row {
  /* flex: 1; */
  /* width: 350rpx;
  height: 500rpx; */
  position: relative;
  background-color: #f5f5f5;
  border: 1rpx solid #ddd;
}
/* .canvasBox{
  height: 500rpx;
  position: relative;
  background-color: #f5f5f5;
} */
.pos-center {
  position: absolute;
  top:0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
.bigCircle{
  width: 320rpx;
  height: 320rpx;
  border-radius: 50%;
  background-color: #313250;
}
.littleCircle{
  width: 256rpx;
  height: 256rpx;
  border-radius: 50%;
  background-color: #171a3b;
}
.canvas{
  width: 320rpx;
  height: 320rpx;
  z-index: 99;
}
```
* js
```
import Canvas from '../../utils/canvas.js'
Page({
  ...Canvas.options, // 通过解构赋值，将共有文件中的方法和数据传递到要使用方法的文件
  /**
   * 页面的初始数据
   */
  data: {
    canvasList: [{
      id: 'circle-one',
      percent: 60
    },
    {
      id: 'circle-second',
      percent: 45
    },
    {
      id: 'circle-three',
      percent: 30
    }],
  },
  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    const _canvasList= this.data.canvasList;
    const len = _canvasList.length;
    for(let i = 0; i < len; i++) {
      this.draw(_canvasList[i].id, _canvasList[i].percent, 1000);
    }
    // this.draw(canvasList[0], this.data.percent2, 1000);
    // this.draw('three', this.data.percent2, 1000);
  }
})
```
```
* <a href="https://blog.csdn.net/weixin_41257563/article/details/83542225" style="color: blue;">To Learn More...</a>

##### 使用wx-charts画图
*  <a href="https://blog.csdn.net/m0_37938910/article/details/80744562" style="color: blue;">参考实例1</a>
*  <a href="https://blog.csdn.net/Che_rish/article/details/78932945" style="color: blue;">参考实例2</a>
*  <a href="https://www.oschina.net/p/wx-charts" style="color: blue;">参考实例3</a>
* 支持图标类型
    * 饼图 pie
    * 圆环图 ring
    * 线图 line
    * 柱状图 column
    * 区域图 area
    * 雷达图 radar

##### 热力图
* inMap
*  <a href="https://juejin.im/post/5a41f3a451882560b76c6d05" style="color: blue;">参考实例1</a>
*  <a href="https://moweide.com/2019/01/10/vue_inaction_toolkits/" style="color: blue;">涉及的Blog</a>

##### 微信小程序里面评论功能
*  <a href="https://www.oschina.net/p/wxcomment" style="color: blue;">参考实例1</a>

##### 微信小程序图片预加载
*  <a href="https://www.oschina.net/p/wxapp-img-loader" style="color: blue;">参考实例1</a>

##### 微信小程序分享相关
* action-sheet 底部弹窗
    * <a href="https://blog.csdn.net/weixin_42567361/article/details/81329926" style="color: blue;">参考实例1</a>
    * <a href="https://gitee.com/ZhongBangKeJi/CRMEB" style="color: blue;">参考实例2</a>

 