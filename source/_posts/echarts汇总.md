---
title: echarts汇总
date: 2022-01-26 11:43:35
categories:
- 前端框架
tags:
---

### echarts汇总
#### setOption
```
  // main.js
  import * as echarts from 'echarts'
  Vue.prototype.$echarts = echarts
  * echarts setOption第二个参数的含义
  chart.setOption(option, notMerge, lazyUpdate)
  * notMerge 可选，是否不跟之前设置的 option 进行合并，默认为 false，即合并
  
  myChart.setOption({...},true)

```

#### ECharts 默认loading
```
  initChart() {
    this.chart = this.$echarts.init(this.$el);
    this.chart.showLoading({
      text: 'loading...',
      color: '#1376fe',
      textColor: '#1376fe',
      // maskColor: 'rgba(0, 0, 0, 0.8)',
      zlevel: 0,
    });
    setTimeout(() => {
      this.chart.hideLoading();
      this.setOptions(this.chartData);
    }, 1000)
  },
```

#### resize
```
  * 每个子组件图表重新初始化一次，只需要在init中加入this.echart.resize
  this.echart.resize()
```

#### echarts设置之stack参数
* stack---数据堆叠，同个类目轴上系列配置相同的stack值后，后一个系列的值会在前一个系列的值上相加
* 解决方法是，stack去掉，或者stack给不同的值
```
 series: [{
  stack: 'total',
 }]

```
