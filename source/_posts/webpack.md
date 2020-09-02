---
title: webpack
date: 2019-05-06 22:00:00
categories:
- tool
tags:
---

#### webpack
##### 有哪些常见 loader 和 plugin，你用过哪些？
1. loader
    * file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
    * url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
    * source-map-loader：加载额外的 Source Map 文件，以方便断点调试
    * image-loader：加载并且压缩图片文件
    * babel-loader：把 ES6 转换成 ES5
    * css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
    * style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
    * eslint-loader：通过 ESLint 检查 JavaScript 代码
2. plugin
    * define-plugin：定义环境变量
    * commons-chunk-plugin：提取公共代码
    * uglifyjs-webpack-plugin：通过UglifyES压缩ES6代码

##### loader 和 plugin 的区别是什么？
1. 不同的作用
   * Loader直译为"加载器"。Webpack将一切文件视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析非JavaScript文件的能力。
    Plugin直译为"插件"。Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
2. 不同的用法
    * Loader在module.rules中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）
    Plugin在plugins中单独配置。 类型为数组，每一项是一个plugin的实例，参数都通过构造函数传入。


##### 如何按需加载代码？
1. Vue UI组件库的按需加载 为了快速开发前端项目，经常会引入现成的UI组件库如ElementUI、iView等，但是他们的体积和他们所提供的功能一样，是很庞大的。 而通常情况下，我们仅仅需要少量的几个组件就足够了，但是我们却将庞大的组件库打包到我们的源码中，造成了不必要的开销。
不过很多组件库已经提供了现成的解决方案，如Element出品的babel-plugin-component和AntDesign出品的babel-plugin-import 安装以上插件后，在.babelrc配置中或babel-loader的参数中进行设置，即可实现组件按需加载了。

2. 单页应用的按需加载 现在很多前端项目都是通过单页应用的方式开发的，但是随着业务的不断扩展，会面临一个严峻的问题——首次加载的代码量会越来越多，影响用户的体验。
通过import(*)语句来控制加载时机，webpack内置了对于import(*)的解析，会将import(*)中引入的模块作为一个新的入口在生成一个chunk。 当代码执行到import(*)语句时，会去加载Chunk对应生成的文件。import()会返回一个Promise对象，所以为了让浏览器支持，需要事先注入Promise polyfill

##### 如何提高构建速度？
1. 多入口情况下，使用CommonsChunkPlugin来提取公共代码
2. 通过externals配置来提取常用库
3. 利用DllPlugin和DllReferencePlugin预编译资源模块 通过DllPlugin来对那些我们引用但是绝对不会修改的npm包来进行预编译，再通过DllReferencePlugin将预编译的模块加载进来。
4. 使用Happypack 实现多线程加速编译
5. 使用webpack-uglify-parallel来提升uglifyPlugin的压缩速度。 原理上webpack-uglify-parallel采用了多核并行压缩来提升压缩速度
6. 使用Tree-shaking和Scope Hoisting来剔除多余代码

##### 转义出的文件过大怎么办？

<a href="https://zhuanlan.zhihu.com/p/44438844" style="color: blue">更多资源</a>

