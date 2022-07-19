---
title: vue3与vue2
date: 2019-07-08 12:04:38
categories:
- 前端框架
tags:
---

#### vue3(指vue-cli3)与vue2差异
* vue3
  * 性能的提升
    * 打包大小减小41%
    * 初次渲染快55%，更新快了33%
    * 内存使用减少54%
  * Composition API
  * 其他特性和新增加的内置组件
  * 更好的 Typescript 支持
* vue2
  * 随着功能的增长，复杂组件的代码变得越来越难以维护
    * Mixin缺点
      * 命名冲突
      * 不清楚暴露出来变量的作用
      * 重用到其它component经常会遇到问题
  * Vue2 对 Typescript 的支持不完善
##### 创建项目，vue3比起vue2简单很多
    * vue create xxx(项目名称) 或者vue ui
        * 选择安装模式
        * ts-sass自己安装过的模板
        * default系统默认
        * Manually select features自己选配置
        * 注意的是
        * 卸载 npm uninstall vue-cli -g
        * npm install -g @vue/cli // 安装cli3.x
        * 更新node到最新版本 npm install -g n(安装模块 这个模块是专门用来管理node.js版本的) n stable(sudo n stable） // 更新node版本
    * <a href="https://juejin.im/post/5bdec6e8e51d4505327a8952" style="color: blue;">想了解more跳转</a>
    * 移动端vue3配置 <a href="https://juejin.im/post/5cbf32bc6fb9a03236393379“ style="color: blue;">想了解more跳转</a>

```
// vue.config.js的配置
// vue.config.js
module.exports = {
    // 选项...
    // 当使用基于 HTML5 history.pushState 的路由时；
    // 当使用 pages 选项构建多页面应用时。
    baseUrl:"",
    // 当运行 vue-cli-service build 时生成的生产环境构建文件的目录。注意目标目录在构建之前会被清除 (构建时传入 --no-clean 可关闭该行为)。
    outputDir:"webApp",
    // 放置生成的静态资源 (js、css、img、fonts) 的 (相对于 outputDir 的) 目录。
    assetsDir:"assets",
    // 指定生成的 index.html 的输出路径 (相对于 outputDir)。也可以是一个绝对路径。
    indexPath:"index.html",
    // 默认情况下，生成的静态资源在它们的文件名中包含了 hash 以便更好的控制缓存。然而，这也要求 index 的 HTML 是被 Vue CLI 自动生成的。如果你无法使用 Vue CLI 生成的 index HTML，你可以通过将这个选项设为 false 来关闭文件名哈希。
    filenameHashing:true,
    // 多页面
    pages:undefined,
    // 编译警告
    lintOnSave:false,
    // 是否使用包含运行时编译器的 Vue 构建版本。设置为 true 后你就可以在 Vue 组件中使用 template 选项了，但是这会让你的应用额外增加 10kb 左右。
    runtimeCompiler:false,
    // 默认情况下 babel-loader 会忽略所有 node_modules 中的文件。如果你想要通过 Babel 显式转译一个依赖，可以在这个选项中列出来。
    transpileDependencies:[],
    // 如果你不需要生产环境的 source map，可以将其设置为 false 以加速生产环境构建。
    productionSourceMap:false,
    // 设置生成的 HTML 中 <link rel="stylesheet"> 和 <script> 标签的 crossorigin 属性。需要注意的是该选项仅影响由 html-webpack-plugin 在构建时注入的标签 - 直接写在模版 (public/index.html) 中的标签不受影响。
    crossorigin:undefined,
    // 在生成的 HTML 中的 <link rel="stylesheet"> 和 <script> 标签上启用 Subresource Integrity (SRI)。如果你构建后的文件是部署在 CDN 上的，启用该选项可以提供额外的安全性。需要注意的是该选项仅影响由 html-webpack-plugin 在构建时注入的标签 - 直接写在模版 (public/index.html) 中的标签不受影响。另外，当启用 SRI 时，preload resource hints 会被禁用，因为 Chrome 的一个 bug 会导致文件被下载两次。
    integrity:false,
    // 反向代理
    devServer:{
        // devServer: {
        //     proxy: {
        //       '/api': {
        //         target: '1',
        //         ws: true,
        //         changeOrigin: true
        //       }
        //     }
        // }
    }
}
```
    * cd xxx(项目名称)
    * yarn serve

##### vue开发技巧
* <a href="https://juejin.im/post/5ce3b519f265da1bb31c0d5f#heading-1" style="color: blue;">To Learn More</a> 
* 长列表性能优化
    * vue会通过Object.defineProperty对数据进行劫持，来实现视图响应数据的变化，有些时候我们的组件就是纯粹的数据展示，不会有任何改变，就不需要vue来劫持我们的数据，在大量数据展示的情况下，能够很明显的减少组件初始化的时间，如何禁止vue劫持我们的数据呢？
    **可以通过object.freeze方法来冻结一个对象，一旦被冻结的对象就再也不能被修改了**
```
export default {
  data: () => ({
    users: {}
  }),
  async created() {
    const users = await axios.get("/api/users");
    this.users = Object.freeze(users);
  }
};
// 冻结了users的值，引用不会被冻结，当需要reactive数据的时候，可以重新给users赋值
export default {
  data: () => ({
    users: []
  }),
  async created() {
    const users = await axios.get("/api/users");
    this.users = Object.freeze(users);
  },
  methods:{
    // 改变值不会触发视图响应
    this.users[0] = newValue
    // 改变引用依然会触发视图响应
    this.users = newArray
  }
};
```

##### vue中8种组件的通信方式
* <a href="https://juejin.im/post/5d267dcdf265da1b957081a3" style="color: blue;">To Learn More</a> 
* props/$emit
  * 父组件通过props的方式向子组件传递数据，通过$emit子组件向父组件通信
  **props只可以从上一级组件传递到下一级组件(父子组件)，即单向数据流。且props只读，不可被修改，所有修改都会失效并警告**
```
// section父组件
<template>
  <div class="section">
    <com-article :articles="articleList"></com-article>
  </div>
</template>

<script>
import comArticle from './test/article.vue'
export default {
  name: 'Section',
  components: { 
    comArticle 
  },
  data () {
    return {
      articleList: ['活着', '生活是一种奢侈', '小王子']
    }
  }
}
</script>

// 子组件 article.vue
<template>
  <div>
    <span v-for="(item, index) in articles" :key="index">{{item}}</span>
  </div>
</template>

<script>
export default {
  props: ['articles'] // 获取父组件的数据
}
</script>

```
* 子组件向父组件传值
  * $emit绑定一个自定义事件，当指令执行时，就会将参数arg传递给父组件，父组件通过v-on监听并接收参数
```
// 父组件中
<template>
  <div class="section">
    <com-article :articles="articleList" @onEmitIndex="onEmitIndex"></com-article>
    <p>{{currentIndex}}</p>
  </div>
</template>

<script>
import comArticle from './test/article.vue'
export default {
  name: 'Section',
  components: { 
    comArticle 
  },
  data () {
    return {
      currentIndex: -1,
      articleList: ['活着', '生活是一种奢侈', '小王子']
    }
  },
  methods: {
    // 监听子组件传递的参数
    onEmitIndex (idx) {
      this.currentIndex = idx;
    }
  }
}
</script>

<template>
  <div>
    <div v-for="(item, index) in articles" :key="index" @click="emitIndex(index)">{{item}}</div>
  </div>
</template>

<script>
export default {
  props: ['articles'],
  methods: {
    // 派发到父组件的事件
    emitIndex(index) {
      this.$emit('onEmitIndex', index)
    }
  }
}
</script>

```

* $children/$parent
  * 通过$children和$parent可以访问组件的实例，代表可以访问组件的所有方法和data
  **在#app上拿$parent得到的是new Vue()的实例，在这实例上再拿$parent得到的是undefined，而在最底层的子组件拿$children是个空数组。也要注意得到$parent和$children的值不一样，$children的值是数组，而$parent是个对象**
* 父子组件之间的通信， 而使用props进行父子组件通信更加普遍; 二者皆不能用于非父子组件之间的通信。