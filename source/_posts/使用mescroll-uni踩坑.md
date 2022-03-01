---
title: 使用mescroll-uni踩坑
date: 2022-03-01 10:49:08
categories:
- 前端框架、uniapp
tags:
---

### uniapp使用mescroll-uni
* <a href="http://www.mescroll.com/uni.html#options" style="color: blue;">To Learn More</a> 
#### tabs切换公用mescroll-uni
* 初始化第一次定义不执行上拉刷新、下拉加载
```
  downOption: {
    auto: false, // 默认第一次不加载
  },
  upOption: {
    auto: false,
    page: {
      num: 0, //当前页 默认0,回调之前会加1; 即callback(page)会从1开始， 这里定义必须是0，不然切换tab重置this.mescroll.resetUpScroll() 之后，从第二页开始执行
      size: 10,
    },
  }
```
* 切换tab初始化方法
```
  tabChage(item) {
    // 重置下拉刷新
    if (this.mescroll) {
      this.mescroll.scrollTo(0, 300);
    }
    // 重置列表为第一页 (自动执行 page.num=1, 再触发upCallback方法 )
    this.mescroll.resetUpScroll();
  }
```

* 切换tab包裹了除列表之外的其余部分（不需要重新渲染）但可以滑动，配合v-if实现，及动态高度
* top设置距离顶部高度
```
  <view
    :style="{ height: 'calc(100vh' + -550 + 'rpx)' }"
    v-if="listData.length || pieChart"
  >
    <mescroll-uni
      ref="mescrollRef"
      top="185"
      @init="mescrollInit"
      @down="downCallback"
      :up="upOption"
      :down="downOption"
      @up="upCallback"
    >
    </mescroll-uni>
  </view>
```