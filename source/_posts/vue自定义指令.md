---
title: vue自定义指令
date: 2021-01-05 11:49:15
categories:
- 前端框架
tags:
---

### [vue自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)
#### vue自定义指令有全局注册和局部注册两种方式
* 全局注册
```
Vue.directive(id, [definition]) 
```
* 局部注册
```
<!-- 批量注册 -->
import copy from './copy'
import longpress from './longpress'
// 自定义指令
const directives = {
  copy,
  longpress,
}

export default {
  install(Vue) {
    Object.keys(directives).forEach((key) => {
      Vue.directive(key, directives[key])
    })
  },
}
```
* 注册之后使用在main.js
```
<!-- main.js使用 -->
import Vue from 'vue'
import Directives from './directives/index'
Vue.use(Directives)
```
#### 指令函数中提供的几个钩子函数（可选）：
* bind: 只调用一次，指令第一次绑定到元素时调用，可以定义一个在绑定时执行一次的初始化动作
* inserted: 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）
* update: 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值
* componentUpdated: 被绑定元素所在模板完成一次更新周期时调用
* unbind: 只调用一次， 指令与元素解绑时调用

#### 常使用的自定义指令
* 防抖指令 v-debounce
  * 防止按钮在短时间内被多次点击，使用防抖函数限制规定时间内只能点击一次
```
const debounce = {
  inserted: function (el, binding) {
    let timer;
    <!-- keyup -->
    el.addEventListener('click', () => {
      if (timer) {
        clearTimeout(timer)
      }
      timer = setTimeout(() => {
        binding.value()
      }, 1000)
    })
  },
}
export default debounce;

<!-- dom中使用 -->
<template>
  <button v-debounce="debounceClick">防抖</button>
</template>

<script> 
export default {
  methods: {
    debounceClick () {
      console.log('只触发一次')
    }
  }
}
</script>
```

* 实现一个图片懒加载指令，只加载浏览器可见区域的图片
  * 实现一个图片懒加载指令，只加载浏览器可见区域的图片。
```
const LazyLoad = {
  // install方法
  install(Vue, options) {
    const defaultSrc = options.default
    Vue.directive('lazy', {
      bind(el, binding) {
        LazyLoad.init(el, binding.value, defaultSrc)
      },
      inserted(el) {
        if (IntersectionObserver) {
          LazyLoad.observe(el)
        } else {
          LazyLoad.listenerScroll(el)
        }
      },
    })
  },
  // 初始化
  init(el, val, def) {
    el.setAttribute('src', val)
    el.setAttribute('src', def)
  },
  // 利用IntersectionObserver监听el
  observe(el) {
    var io = new IntersectionObserver((entries) => {
      const realSrc = el.dataset.src
      if (entries[0].isIntersecting) {
        if (realSrc) {
          el.src = realSrc
          el.removeAttribute('src')
        }
      }
    })
    io.observe(el)
  },
  // 监听scroll事件
  listenerScroll(el) {
    const handler = LazyLoad.throttle(LazyLoad.load, 300)
    LazyLoad.load(el)
    window.addEventListener('scroll', () => {
      handler(el)
    })
  },
  // 加载真实图片
  load(el) {
    const windowHeight = document.documentElement.clientHeight
    const elTop = el.getBoundingClientRect().top
    const elBtm = el.getBoundingClientRect().bottom
    const realSrc = el.dataset.src
    if (elTop - windowHeight < 0 && elBtm > 0) {
      if (realSrc) {
        el.src = realSrc
        el.removeAttribute('src')
      }
    }
  },
  // 节流
  throttle(fn, delay) {
    let timer
    let prevTime
    return function (...args) {
      const currTime = Date.now()
      const context = this
      if (!prevTime) prevTime = currTime
      clearTimeout(timer)

      if (currTime - prevTime > delay) {
        prevTime = currTime
        fn.apply(context, args)
        clearTimeout(timer)
        return
      }

      timer = setTimeout(function () {
        prevTime = Date.now()
        timer = null
        fn.apply(context, args)
      }, delay)
    }
  },
}

export default LazyLoad;
<!-- dom中使用 -->
<img v-LazyLoad="xxx.jpg" />
```
[分享8个非常实用的Vue自定义指令](https://mp.weixin.qq.com/s/DwmUqgD-y3qXKZ87-RdEDA)