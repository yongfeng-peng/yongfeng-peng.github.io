---
title: uniapp
date: 2021-12-02 10:45:27
categories:
- 前端框架
tags:
---

### uniapp项目总结
#### 解决手机软键盘弹出挤压页面变形问题
```
<!-- 统一处理模版页面按钮定位 -->
export const templateHeight = () => {
  return uni.getSystemInfoSync().windowHeight - uni.upx2px(100);
}
<!-- 页面布局 -->
<view class="template-wrap" :style="{height: height + 'px'}">
  <view class="template-wrap-con">
    内容部分
  </view>
  <view class="bottom"">
    底部固定按钮部分
  </view>
</view>
<!-- css部分 -->
.template-wrap {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  .template-wrap-con {
    flex-grow: 1;
    overflow: overlay;
  }
}
```

#### css布局
* ios个别版本对fixed的属性的支持性不好，需要用absolute替代
* input 的 placeholder会出现文本位置偏上的时候
  * input 的placeholder会出现文本位置偏上的情况：PC端设置line-height等于height能够对齐，而移动端仍然是偏上，解决是设置line-height：normal

#### 使用uview组件
```
<!-- iOS 15 搜索框里多一个搜索图标怎么破 -->
[type="search"]::-webkit-search-decoration {  
  display: none;  
}
```
```
<!-- iOS picker 日期 初始化日期无效 修改组件 与 uniapp 日期兼容有点差别 -->
<!-- 格式化时间，在IE浏览器(uni不存在此情况)，无法识别日期间的"-"间隔符号 -->
<!-- 处理IOS不显示问题 -->
<!-- let fdate = this.defaultTime.replace(/\-/g, '/'); // 无需把-改为/ -->
let fdate = this.defaultTime;

<!-- uniapp iOS手机日期不识别- -->
<!-- 把yyy-mm-dd hh:mm:ss 格式ios不识别 把yyy-mm-dd hh:mm:ss 格式转换成 yyy/mm/dd hh:mm:ss -->
export const iosDataFilter = (time) => {
  return time.replace(/-/g, '/');
}
```

#### 复制粘贴(直接使用api 在手机app端 样式问题)
```
export const copyHandler = (e) => {
<!-- 直接传入可复制文本，无效 -->
  const { name } = e.currentTarget.dataset;
  let result;
  let textarea = document.createElement('textarea');
  textarea.value = name;
  textarea.readOnly = 'readOnly';
  document.body.appendChild(textarea);
  textarea.select(); // 选中文本内容
  textarea.setSelectionRange(0, name.length); // 设置选定区的开始和结束点
  uni.showToast({
    icon: 'none',
    title: '复制成功'
    
  })
  result = document.execCommand('copy'); //将当前选中区复制到剪贴板
  textarea.remove();
  <!-- 
  uni.setClipboardData({
    data: name,
    success: () => {
        uni.showToast({
          icon: 'none',
          title: '复制成功'
        })
      }
  });
  -->
}
```

#### js方法
```
<!-- 调用async方法 test() -->
  test().then(res=>{
    console.log(res)
  }).catch(err=>{
    console.log(err)
  });
 
<!-- 数据没三位使用,分隔 -->
  numSplit(value) {
    if(value != '-') {
      let val = value && Number(value);
      return val && val.toFixed(2).replace(/(\d)(?=(\d{3})+\.)/g, '$1,');
    }
    return 0;
  }
```
