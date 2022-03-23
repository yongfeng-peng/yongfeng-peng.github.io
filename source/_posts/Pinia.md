---
title: Pinia
date: 2022-03-22 16:07:51
categories:
- tool
tags:
---

### Pinia
* <a href="https://pinia.vuejs.org/" style="color: blue;">官方文档Pinia</a>
* <a href="https://github.com/vuejs/pinia" style="color: blue;">git地址</a>
* <a href="https://blog.csdn.net/qq1195566313/category_11672479.html" style="color: blue;">～more</a>
#### Pinia.js 
* 有如下特点：
  * 全局状态管理工具
  * 完整的 ts 的支持；
  * 足够轻量，压缩后的体积只有1kb左右;
  * 去除 mutations，只有 state，getters，actions；
  * actions 支持同步和异步；
  * 代码扁平化没有模块嵌套，只有 store 的概念，store 之间可以自由使用，每一个store都是独立的
  * 无需手动添加 store，store 一旦创建便会自动添加；
  * 支持Vue3 和 Vue2
* 起步 安装
  * yarn add pinia
  * npm install pinia
* 引入注册Vue3
```
import { createApp } from 'vue'
import App from './App.vue'
import {createPinia} from 'pinia'
const store = createPinia()
let app = createApp(App)
app.use(store)
app.mount('#app')
```
* Vue2 使用
```
import { createPinia, PiniaVuePlugin } from 'pinia'
Vue.use(PiniaVuePlugin)
const pinia = createPinia()
new Vue({
  el: '#app',
  // other options...
  // ...
  // note the same `pinia` instance can be used across multiple Vue apps on
  // the same page
  pinia,
})
```
#### 初始化仓库Store
* 新建一个文件夹Store
* 新建文件[name].ts （存储是使用定义的defineStore()，并且它需要一个唯一的名称，作为第一个参数传递）
  ```
  // store-namespace.ts
  export const enum Names {
    Test = 'TEST'
  }
  ```
* 定义仓库Store
  * store引入（这个名称，也称为id，是必要的，Pania 使用它来将商店连接到 devtools。将返回的函数命名为use...是可组合项之间的约定，以使其使用习惯）
  ```
  import { defineStore } from 'pinia'
  import { Names } from './store-namespce'
  export const useTestStore = defineStore(Names.Test, {
  
  })
  ```
* 定义值
  * State 箭头函数 返回一个对象 在对象里面定义值
  ```
  import { defineStore } from 'pinia'
  import { Names } from './store-namespce'
  
  export const useTestStore = defineStore(Names.Test, {
      state:()=>{
          return {
              current:1
          }
      },
      //类似于computed 可以帮我们去修饰我们的值
      getters:{
  
      },
      //可以操作异步 和 同步提交state
      actions:{
  
      }
  })
  ```
  * .vue使用
  ```
    <template>
      <div>
        pinia: {{ test.current }} ----- {{ test.name }}
      </div>
    </template>

    <script setup lang="ts">
    import { useTestStore } from './store'
    const test = useTestStore();
    </script>
  ```
#### State
* State 是允许直接修改值的 例如current++
* 批量修改State的值(在他的实例上有$patch方法可以批量修改多个值)
* 批量修改函数形式 - 推荐使用函数形式 可以自定义修改逻辑
* 通过原始对象修改整个实例 - $state您可以通过将store的属性设置为新对象来替换store的整个状态 - 缺点就是必须修改整个对象的所有属性
* 通过actions修改 - 定义Actions - 在actions 中直接使用this就可以指到state里面的值
```
import { defineStore } from 'pinia'

import { Names } from './store-name'

export const useTestStore =  defineStore(Names.Test, {
  state:()=>{
      return {
        current: 1,
        name: 'pinia'
      }
  },
  //类似于computed 可以帮我们去修饰我们的值
  getters:{

  },
  //可以操作异步 和 同步提交state
  actions:{
    setCurrent (num: number): void {
      // this.current++; // 不使用箭头函数，this指向有问题
      this.current = num;
    }

  }
});
```
* 使用方法直接在实例调用
```
<template>
  <div>
    <div>
      <button @click="Add">+</button>
    </div>
    pinia: {{ test.current }} ----- {{ test.name }}
  </div>
</template>

<script setup lang="ts">
import { useTestStore } from './store'
let test = useTestStore();
// 修改值
const Add = () => {
  // 1 直接修改
  // test.current++
  // 2
  // test.$patch({
  //   current:200,
  //   name: 300
  // })
  // 3 推荐使用 自定义逻辑
  // test.$patch((state)=>{
  //     state.current++;
  //     state.name = 'pini111'
  // })
  // 4
  // test.$state = {
  //     current: 10,
  //     name: '1111'
  // }  
  // 5
  test.setCurrent(567);
}
</script>

<style lang="less" scoped>

</style>
```
#### 解构store
* Pinia是不允许直接解构是会失去响应性的
* 差异对比
  * 修改Test current 解构完之后的数据不会变
  * 而源数据是会变的
* 解决方案可以使用 storeToRefs
  * 其原理跟toRefs 一样的给里面的数据包裹一层toref
  * 源码 通过toRaw使store变回原始数据防止重复代理
  * 循环store 通过 isRef isReactive 判断 如果是响应式对象直接拷贝一份给refs 对象 将其原始对象包裹toRef 使其变为响应式对象
```
<template>
  <div>
    <div>origin value {{test.current}}</div>
    <div>
      <button @click="Add">+</button>
    </div>
    pinia: {{ current }} ----- {{ name }}
  </div>
</template>

<script setup lang="ts">
import { useTestStore } from './store'
import { storeToRefs } from 'pinia'
 
let test = useTestStore();
let { current, name} = storeToRefs(test);
// 修改值
// 直接修改
const Add = () => {
  // console.log(test);
  // test.current++;
  current.value++
}
</script>

<style lang="less" scoped>

</style>
```

#### Actions,getters
* Actions（支持同步异步）
  * 同步 直接调用即可
  * 异步 可以结合async await 修饰
  * 多个action互相调用getLoginInfo  setName
* getters
  *  使用箭头函数不能使用this this指向已经改变指向undefined 修改值请用state
  * 主要作用类似于computed 数据修饰并且有缓存
  ```
  getters:{
    newPrice:(state)=>  `$${state.user.price}`
  },
  ```
  * 普通函数形式可以使用this
  * getters 互相调用

```
import { defineStore } from 'pinia'

import { Names } from './store-name'

type User = {
  name: string,
  age: number
};

// 同步修改
let result:User = {
  name: 'pinia',
  age: 4
};

// 模拟异步
const Login = ():Promise<User> => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({
        name: 'pinia',
        age: 5
      })
    }, 2000);
  })
}

export const useTestStore =  defineStore(Names.Test, {
  state:() => {
      return {
        current: 1,
        user: <User>{},
        name: '凤'
      }
  },
  // 类似于computed 可以帮我们去修饰我们的值，可以相互调用
  getters: {
    newName():string {
      return `$-${this.name}---${this.getUserAge}`
    },
    getUserAge():number {
      return this.user.age
    }
  },
  //可以操作异步 和 同步提交state，  可以相互调用
  actions: {
    setCurrent (num: number): void {
      // this.current++; // 不使用箭头函数，this指向有问题
      this.current = num;
    },
    // setUser() {
    //   this.user = result;
    // },
    async setUser() {
      const result = await Login();
      this.user = result;
      this.setName('凤凤');
    },
    setName(name: string) {
      this.name = name;
    },

  }
});
```

```
<template>
  <div>
    <p>action-user: {{ Test.user }}</p>
    <hr/>
    <p>action-name: {{ Test.name }}</p>
    <hr/>
    <p>getters:{{ Test.newName }}</p>
    <hr/>
    <button @click="change">change</button>
  </div>
</template>

<script setup lang="ts">
import { useTestStore } from './store'
const Test = useTestStore();
const change = () => {
  Test.setUser();
}
</script>

<style lang="less" scoped>

</style>
```

* store-name.ts
```
export const enum Names {
  Test = 'TEST'
}
```

#### API
* $reset - 重置store到他的初始状态
```
state: () => ({
  user: <Result>{},
  name: "default",
  current: 1
}),
```
* Vue 例如我把值改变到了10
```
const change = () => {
  Test.current++
}
```
* 调用$reset();
  * 将会把state所有值 重置回 原始状态
```
const reset = () => {
  Test.$reset();
};
```

* 订阅state的改变 - 类似于Vuex 的abscribe  只要有state 的变化就会走这个函数
```
Test.$subscribe((args,state)=>{
   console.log(args,state);
   
})
```
  *  第二个参数 - 如果你的组件卸载之后还想继续调用请设置第二个参数
  ```
   Test.$subscribe((args,state)=>{
      console.log('=========', args);
      console.log('=========', state);
    },{
      detached: true
    })
  ```
* 订阅Actions的调用 - 只要有actions被调用就会走这个函数
  ```
    Test.$onAction((args)=>{
      console.log(args);
      args.after(() => {
        console.log('after');
      })
    })
  ```

#### pinia插件
* pinia 和 vuex 都有一个通病 页面刷新状态会丢失
* 写一个pinia 插件缓存他的值
```
  const __piniaKey = '__PINIAKEY__'
  
  type OptPinia = Partial<Options>
  
  const setStorage = (key: string, value: any): void => {
    localStorage.setItem(key, JSON.stringify(value))
  }
  
  const getStorage = (key: string) => {
    return (localStorage.getItem(key) ? JSON.parse(localStorage.getItem(key) as string) : {})
  }
  
  const piniaPlugin = (options: OptPinia) => {
    return (context: PiniaPluginContext) => {
      const { store } = context;
      const data = getStorage(`${options?.key ?? __piniaKey}-${store.$id}`)
      store.$subscribe(() => {
        etStorage(`${options?.key ?? __piniaKey}-${store.$id}`, toRaw(store.$state));
      })
      return {
        ...store.$state,
        ...data
      }
    }
  }
  const pinia = createPinia()
  pinia.use(piniaPlugin({
    key: "pinia"
  }))
```