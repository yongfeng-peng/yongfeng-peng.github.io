---
title: SEO
date: 2022-07-20 17:05:34
categories:
- 
tags:
---

### SEO 搜索引擎优化
#### 最基本条件
  * 多页面 ---- （蜘蛛）抓取
  * 页面含有（蜘蛛）抓取的内容
  * title、描述、关键词

#### vue解决SEO方案
* 方式1: 预渲染
  * vue插件： prerender-spa-plugin
  * vue.config.js 配置 ![Image text](/../images/vue-seo.jpg) (注：有多少个路由、需要配置多少个)
  ```
  const path = require('path');
  const PrerenderSPAPlugin = require('prerender-spa-plugin');
  module.exports = {
    publicPath: './',
    configureWebpack: {
      plugins: [
        new PrerenderSPAPlugin({
          staticDir: path.join(__dirname, 'dist'),
          routes: [
            '/',
            '/about',
            '/contact'
          ]
        })
      ]
    }
  };
  ```
  * 解决title、描述、关键词插件：vue-meta-info
  ```
  // main.js
  import MetaInfo from 'vue-meta-info'
  Vue.use(MetaInfo)
  // 页面使用
  metaInfo: {
    title: '',
    meta: [{
      name: '',
      content: ''
    }]
  }
  ```
* 不足：如果有很多详情页需要seo，预渲染就不合适，动态去改变title、描述、关键字是无效的
* 适合：项目某几个页面做seo

* 方式2: 服务端渲染
  * Nuxt.js 处理流程
    * npm run build
    * 将打包完成的文件单独拷贝出来
      * nuxt.config
      * .nuxt
      * package.json
      * static
    * 将4个文件拷贝到服务器，服务器安装node环境
      * npm install
    * 运行 npm run start
    * nginx设置代理

#### vue项目打包上线
  * 打包路径 vue.config.js publicPath: './'
  * 路由模式：history
    * 前端如果自己测试项目，hash
    * 项目上线要求是 history模式，重定向
