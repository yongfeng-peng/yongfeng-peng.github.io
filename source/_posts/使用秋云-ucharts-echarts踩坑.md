---
title: 使用秋云 ucharts echarts踩坑
date: 2022-03-02 11:07:10
categories:
- 前端框架、uniapp
tags:
---

### uniapp使用秋云 ucharts echarts 高性能跨全端图表组件
* <a href="https://ext.dcloud.net.cn/plugin?id=271" style="color: blue;">To Learn More</a> 
* 依赖uniapp的vue-cli项目：请将uni-modules目录复制到src目录，即src/uni_modules。（请升级uniapp依赖为最新版本）

#### 自定义echarts参数
* 使用eopts 但参数不能包含函数
```
  <qiun-data-charts
    :style="{ height: height, width: width }"
    type="area"
    ref="pieChart"
    :eopts="lineEopts"
    :opts="{yAxis:{data:[{min:0}]},extra:{area:{type:'curve',addLine:true,gradient:true}}}"
    :chartData="chartData" 
    :echartsH5="true" 
    :echartsApp="true"
    tooltipFormat="lineTooltip"
  />
```

#### 自定义tooltipFormat格式
* 在配置文件config-echarts.js里面定义对应的formatter 函数
```
  <qiun-data-charts
    :style="{ height: height, width: width }"
    type="area"
    ref="pieChart"
    :eopts="lineEopts"
    :opts="{yAxis:{data:[{min:0}]},extra:{area:{type:'curve',addLine:true,gradient:true}}}"
    :chartData="chartData" 
    :echartsH5="true" 
    :echartsApp="true"
    tooltipFormat="lineTooltip"
  />

  // tooltip排序
  function compare(property) {
    return (arr, arr2) => {
      let a = arr[property];
      let b = arr2[property];
      return b - a;
    };
  }

  const cfe = {
    "formatter":{
      lineTooltip: (res) => {
        let result = '';
        let _res = res.sort(compare("value"));
        for (let i in _res) {
          if (i == 0) {
            result += res[i].axisValueLabel
          }
          let value = '--'
          if (_res[i].data !== null) {
            value = _res[i].data;
          }
          // #ifdef H5
          result += '\n' + _res[i].seriesName + '：' + value;
          // #endif
          
          // #ifdef APP-PLUS
          result += '<br/>' + _res[i].marker + _res[i].seriesName + '：' + value;
          // #endif
        }
        return result;
     },
  }
```

#### 区域图设置渐变,自定义颜色color、graphicColor
```
initColor() {
  this.colorList = this.color.map((item, index) => {
    return  {
      color: item,
      areaStyle: {
        color: {
          type: 'linear',
          x: 0,
          y: 0,
          x2: 0,
          y2: 1,
          colorStops: [{
            offset: 0,
            color: this.graphicColor[index] // 0% 处的颜色
          }, {
            offset: 0.4,
            color: this.graphicColor[index] // 100% 处的颜色
          }, {
            offset: 1,
            color: "#ffffff",
          }],
          global: false // 缺省为 false
        }
      }
    }
  })
},
```


