---
title: Router
date: 2022-03-23 16:47:59
categories:
- tool
tags:
---

### vue-router
#### router 路由
* 应为vue是单页应用不会有那么多html 让我们跳转 所有要使用路由做页面的跳转
* Vue 路由允许我们通过不同的 URL 访问不同的内容。通过 Vue 可以实现多视图的单页Web应用
* 构建前端项目
```
npm init vue@latest
//或者
npm init vite@latest
```
* <span style="color: red;">使用Vue3 安装对应的router4版本</span>
```
npm install vue-router@4
```
* <span style="color: red;">使用Vue2安装对应的router3版本</span>
* 在src目录下面新建router 文件 然后在router 文件夹下面新建 index.ts
```
//引入路由对象
import { createRouter, createWebHistory, createWebHashHistory, createMemoryHistory, RouteRecordRaw } from 'vue-router'
//vue2 mode history vue3 createWebHistory
//vue2 mode  hash  vue3  createWebHashHistory
//vue2 mode abstact vue3  createMemoryHistory
 
//路由数组的类型 RouteRecordRaw
// 定义一些路由
// 每个路由都需要映射到一个组件。
const routes: Array<RouteRecordRaw> = [{
  path: '/',
  component: () => import('../components/a.vue')
},{
  path: '/register',
  component: () => import('../components/b.vue')
}]
 
const router = createRouter({
  history: createWebHistory(),
  routes
})
//导出router
export default router
```
* router-link<a href="https://router.vuejs.org/zh/guide/#router-link" style="color: blue;"> # </a>
  * 没有使用常规的 a 标签，而是使用一个自定义组件 router-link 来创建链接。这使得 Vue Router 可以在不重新加载页面的情况下更改 URL，处理 URL 的生成以及编码
* router-view<a href="https://router.vuejs.org/zh/guide/#router-view" style="color: blue;"> # </a>
  * router-view 将显示与 url 对应的组件。你可以把它放在任何地方，以适应你的布局

```
<template>
  <div>
    <h1>router</h1>
    <div>
    <!--使用 router-link 组件进行导航 -->
    <!--通过传递 `to` 来指定链接 -->
    <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
      <router-link tag="div" to="/">跳转a</router-link>
      <router-link tag="div" style="margin-left:200px" to="/register">跳转b</router-link>
    </div>
    <hr />
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
</template>
```
* 最后在main.ts 挂载
```
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
createApp(App).use(router).mount('#app')
```

#### 命名路由-编程式导航
* 命名路由
  * 除了 path 之外，你还可以为任何路由提供 name。这有以下优点：
    * 没有硬编码的 URL
    * params 的自动编码/解码
    * 防止你在 url 中出现打字错误
    * 绕过路径排序（如显示一个）
```
const routes:Array<RouteRecordRaw> = [
    {
        path:"/",
        name:"Login",
        component:()=> import('../components/login.vue')
    },
    {
        path:"/reg",
        name:"Reg",
        component:()=> import('../components/reg.vue')
    }
]
```
* router-link跳转方式需要改变 变为对象并且有对应name
```
<h1>router</h1>
<div>
  <router-link :to="{name:'Login'}">Login</router-link>
  <router-link style="margin-left:10px" :to="{name:'Reg'}">Reg</router-link>
</div>
<hr />
```
* 编程式导航
  * 除了使用 <router-link> 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现
  * 字符串模式
  ```
  import { useRouter } from 'vue-router'
  const router = useRouter()
  const toPage = () => {
    router.push('/reg')
  }
  ```
  * 对象模式
  ```
  import { useRouter } from 'vue-router'
  const router = useRouter()
  
  const toPage = () => {
    router.push({
      path: '/reg'
    })
  }
  ```
  * 命名式路由模式
  ```
  import { useRouter } from 'vue-router'
  const router = useRouter()
  
  const toPage = () => {
    router.push({
      name: 'Reg'
    })
  }
  ```
  * a标签跳转
    * 直接通过a href也可以跳转但是会刷新页面  
  ```
 <a href="/reg">rrr</a>
  ```

#### 历史记录
* replace的使用
  * 采用replace进行页面的跳转会同样也会创建渲染新的Vue组件，但是在history中其不会重复保存记录，而是替换原有的vue组件
* router-link 使用方法
```
<router-link replace to="/">Login</router-link>
<router-link replace style="margin-left:10px" to="/reg">Reg</router-link>
```
* 编程式导航
```
<button @click="toPage('/')">Login</button>
<button @click="toPage('/reg')">Reg</button>
// js
import { useRouter } from 'vue-router'
const router = useRouter()
const toPage = (url: string) => {
  router.replace(url)
}
```
* 横跨历史
  * 该方法采用一个整数作为参数，表示在历史堆栈中前进或后退多少步
```
<button @click="next">前进</button>
 <button @click="prev">后退</button>
const next = () => {
  //前进 数量不限于1
  router.go(1)
}
const prev = () => {
  //后退
  router.back()
}
```

#### 路由传参
* Query路由传参
  * 编程式导航 使用router push 或者 replace 的时候 改为对象形式新增query 必须传入一个对象
```
const toDetail = (item: Item) => {
  router.push({
      path: '/reg',
      query: item
  })
}
```
* 接受参数
  * <span style="color: red;">使用 useRoute 的 query</span>
```
import { useRoute } from 'vue-router';
const route = useRoute()
<div>ID：{{ route.query?.id }}</div>
```
* Params路由传参
  * 编程式导航 使用router push 或者 replace 的时候 改为对象形式并且只能使用name，path无效，然后传入params
```
const toDetail = (item: Item) => {
  router.push({
    name: 'Reg',
    params: item
  })
}
```
* 接受参数
  * <span style="color: red;">使用 useRoute 的 params</span>
```
import { useRoute } from 'vue-router';
const route = useRoute()
<div>品牌：{{ route.params?.name }}</div>
<div>价格：{{ route.params?.price }}</div>
<div>ID：{{ route.params?.id }}</div>
```
* 动态路由传参
  * 很多时候，我们需要将给定匹配模式的路由映射到同一个组件。例如，我们可能有一个 User 组件，它应该对所有用户进行渲染，但用户 ID 不同。在 Vue Router 中，我们可以在路径中使用一个动态字段来实现，我们称之为 路径参数 
  * <span style="color: red;">路径参数 用冒号 : 表示。当一个路由被匹配时，它的 params 的值将在每个组件</span>
```
const routes:Array<RouteRecordRaw> = [
    {
        path:"/",
        name:"Login",
        component:()=> import('../components/login.vue')
    },
    {
        //动态路由参数
        path:"/reg/:id",
        name:"Reg",
        component:()=> import('../components/reg.vue')
    }
]
const toDetail = (item: Item) => {
    router.push({
        name: 'Reg',
        params: {
            id: item.id
        }
    })
}
import { useRoute } from 'vue-router';
import { data } from './list.json'
const route = useRoute()
const item = data.find(v => v.id === Number(route.params.id))
```
* 二者的区别
  * query 传参配置的是 path，而 params 传参配置的是name，在 params中配置 path 无效
  * query 在路由配置不需要设置参数，而 params 必须设置
  * query 传递的参数会显示在地址栏中
  * params传参刷新会无效，但是 query 会保存传递过来的值，刷新不变
  * 路由配置

#### 嵌套路由
  * 一些应用程序的 UI 由多层嵌套的组件组成。在这种情况下，URL 的片段通常对应于特定的嵌套组件结构，例如：
  * 如你所见，children 配置只是另一个路由数组，就像 routes 本身一样。因此，你可以根据自己的需要，不断地嵌套视图
```
const routes: Array<RouteRecordRaw> = [
  {
    path: "/user",
    component: () => import('../components/footer.vue'),
    children: [
      {
        path: "",
        name: "Login",
        component: () => import('../components/login.vue')
      },
      {
        path: "reg",
        name: "Reg",
        component: () => import('../components/reg.vue')
      }
    ]
  },
]
```
* <span style="color: red;">TIPS：不要忘记写router-view</span>
```
<div>
  <router-view></router-view>
  <div>
      <router-link to="/">login</router-link>
      <router-link style="margin-left:10px;" to="/user/reg">reg</router-link>
  </div>
</div>
```

#### 命名视图
  * 命名视图可以在同一级（同一个组件）中展示更多的路由视图，而不是嵌套显示。 命名视图可以让一个组件中具有多个路由渲染出口，这对于一些特定的布局组件非常有用。 命名视图的概念非常类似于“具名插槽”，并且视图的默认名称也是 default
  * 一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 components 配置 (带上 s)
```
import { createRouter, createWebHistory, RouteRecordRaw } from 'vue-router'
const routes: Array<RouteRecordRaw> = [
  {
    path: "/",
    components: {
      default: () => import('../components/layout/menu.vue'),
      header: () => import('../components/layout/header.vue'),
      content: () => import('../components/layout/content.vue'),
    }
  },
]
const router = createRouter({
  history: createWebHistory(),
  routes
})
export default router
```
* 对应Router-view 通过name 对应组件
```
<div>
  <router-view></router-view>
  <router-view name="header"></router-view>
  <router-view name="content"></router-view>
</div>
```
