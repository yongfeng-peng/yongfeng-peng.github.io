---
title: vue项目总结
date: 2022-01-25 10:20:22
categories:
- 前端框架
tags:
---

### vue项目总结
#### vue bus触发$emit 监听$on 关闭$off传值
```
  // main.js
  Vue.prototype.bus=new Vue()

  // 子组件
  this.bus.$emit("planLineDetail", false);

  // 父组件
  mounted() {
    this.bus.$on("planLineDetail", (state, id) => {
      // 逻辑
    }
  });
  
  beforeDestroy() {
    this.bus.$off("planLineDetail");
  },
```
#### vue 混入 (mixin)
* 混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项
```
  // .vue页面使用
  import common from "../mixins/common.js";
  export default {
    mixins: [screenSize, common, ....], // 混入js名称
  }
  // common.js
  export default {
  data(){
    return {
    }
  },
  methods: {
   
  },
}
```
### vue css 外部引入
```
  // 直接页面分离
  <style src="./emergency.scss"  lang="scss" scoped></style>
  // import
  <style lang="scss" scoped>
  @import "./detial.scss";
  </style>
```
### vue 动态组件(component)
```
  <template>
    <div>
      <div class="form-con">
        <component :is="currentView"></component>
      </div>
    </div>
  </template>
  * currentView 已注册组件的名字，或一个组件的选项对象
```
### vue 全屏 Fullscreen
```
  mounted() {
    const that = this;
    window.onresize = () => {
      return (() => {
        this.isFullscreen = this.checkFull(); // 判断是否可以全屏
      })();
    };
  },
  // 判断浏览器是否处于全屏状态 （需要考虑兼容问题）
  function checkFull() {
    //火狐浏览器
    var isFull =
      document.mozFullScreen ||
      document.fullScreen ||
      //谷歌浏览器及Webkit内核浏览器
      document.webkitIsFullScreen ||
      document.webkitRequestFullScreen ||
      document.mozRequestFullScreen ||
      document.msFullscreenEnabled;
    if (isFull === undefined) {
      isFull = false;
    }
    return isFull;
  }
```

