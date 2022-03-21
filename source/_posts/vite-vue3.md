---
title: vite+vue3
date: 2022-03-17 09:59:13
categories:
- 前端框架
tags:
---

### vite
* <a href="https://vitejs.cn/guide/why.html#slow-server-start" style="color: blue;">vite</a>
* <a href="https://blog.csdn.net/qq1195566313/category_11618172.html" style="color: blue;">more</a>
#### 首次执行命令(npm init vite@latest)一直报错
* 解决
  * 修改源地址位淘宝镜像
```
npm config set registry http://registry.npm.taobao.org/
```
* 修改源地址为官方源
```
npm config set registry https://registry.npmjs.org/
```
* 再使用cnpm
* 下载淘宝镜像
```
npm  install -g cnpm --registry==http://registry.npm.taobao.org
```
* 指定淘宝镜像
```
npm config set  registry  http://registry.npm.taobao.org/
```

#### vite的优势
* 冷服务 默认构建目标浏览器是能在script标签上支持原生ESM和原生ESM动态导入
  * <a href="https://zhuanlan.zhihu.com/p/400573436" style="color: blue;">更多ESM</a>
* HMR模块热替换（hot module replacement）速度快 模块热更新（HMR）
* Rollup打包，它使用rollup打包的代码，并且它是预配置的 支持大部分的rollup插件

#### 目录
* public -- 静态文件（不被编译）
* src/assets -- 静态文件（被编译）

#### diff 算法
* 有无key 是否覆盖，及跳过（顺、逆、乱）

#### 相关Ref
 * ref
  * 接受一个内部值并返回一个响应式且可变的 ref 对象。ref 对象仅有一个 .value property，指向该内部值
```
<template>
  <div>
    <button @click="changeHandle">点击</button>
    <div>{{ mes }}</div>
  </div>
</template>
 
<script setup lang="ts">
let mes: string = "初学习vite"
const changeHandle = () => {
  mes = "change msg"
}
</script>
 
<style>
</style>
```
* 以上操作，无法改变mes的值,因为mes并不是响应式的，无法被vue跟踪要改成ref
```
<template>
  <div>
    <button @click="changeHandle">点击</button>
    <div>{{ mes }}</div>
  </div>
</template>
 
<script setup lang="ts">
import {ref, Ref} from 'vue'
let mes:Ref<string> = ref("我是message");
 
const changeHandle = () => {
  mes.value = "change msg"
}
</script>
 
<style>
</style>
// ------------------------------- ts两种方式 ---------------------------------
<template>
  <div>
    <button @click="changeHandle">点击</button>
    <div>{{ mes }}</div>
  </div>
</template>
 
<script setup lang="ts">
import { ref } from 'vue'
let mes = ref<string | number>("我是message")
 
const changeHandle = () => {
  message.value = "change msg"
}
</script>
 
<style>
</style>
```
* isRef
  * 判断是不是一个ref对象
```
import { ref, Ref,isRef } from 'vue'
let message: Ref<string | number> = ref("我是message")
let notRef:number = 123
const changeMsg = () => {
  message.value = "change msg"
  console.log(isRef(message)); //true
  console.log(isRef(notRef)); //false
  
}
```
* shallowRef
  * 创建一个跟踪自身 .value 变化的 ref，但不会使其值也变成响应式的
  * 修改其属性是非响应式的这样是不会改变的

* triggerRef
  * 强制更新页面DOM

* customRef
  * customRef 是个工厂函数要求我们返回一个对象 并且实现 get 和 set 

#### reactive
* 用来绑定复杂的数据类型 例如 对象 数组
* 不可以绑定普通的数据类型这样是不允许 会给我们报错
* 如果用ref去绑定对象 或者 数组 等复杂的数据类型,看源码里面其实也是 去调用reactive,使用reactive 去修改值无须.value
* 数组异步赋值问题
  * 不会变化
```
let person = reactive<number[]>([])
setTimeout(() => {
  person = [1, 2, 3]
  console.log(person);
},1000)
```
* 解决方案1
```
import { reactive } from 'vue'
let person = reactive<number[]>([])
setTimeout(() => {
  const arr = [1, 2, 3]
  person.push(...arr)
  console.log(person);
},1000)
```
* 解决方案2
```
type Person = {
  list?:Array<number>
}
let person = reactive<Person>({
  list:[]
})
setTimeout(() => {
  const arr = [1, 2, 3]
  person.list = arr;
  console.log(person);
},1000)
```
* readonly
  * 拷贝一份proxy对象将其设置为只读
* shallowReactive
  * 只能对浅层的数据 如果是深层的数据只会改变值,不会改变视图

#### to系列
* toRef
  * 原始对象是非响应式的就不会更新视图 数据是会变的
  * 原始对象是响应式的是会更新视图并且改变数据的
* toRefs
  * 批量创建ref对象主要是方便我们解构使用
* toRaw
  * 响应式对象转化为普通对象

#### computed计算属性
* computed用法
  * 计算属性就是当依赖的属性的值发生变化的时候，才会触发他的更改，如果依赖的值，不发生变化的时候，使用的是缓存中的属性值
  * 参数（函数或对象）
```
const name = computed(() => {
  return `${fistName.value} ----- ${lastName.value}`
})
const name = computed({
  get() {
    return `${fistName.value} ----- ${lastName.value}`
  },
  set() {
    return `${fistName.value} ----- ${lastName.value}`
  }
})
```

#### watch侦听器
* watch 需要侦听特定的数据源，并在单独的回调函数中执行副作用
* watch参数
  * 第一个参数监听源(单个数据源、数组、函数)
```
import { ref, watch, reactive } from 'vue';
let message = reactive({
  name: '凤',
  name2: '凤凤',
});
// 只想监听特定的某个属性，可以传入函数
watch(() => message.name, (newVal, oldVal) => {
  console.log('新的', newVal);
  console.log('旧的', oldVal);
}, {
  // deep: true, // reactive类型 是否设置deep 都深度监听
  // immediate: true, // 第一次进入页面是否执行
})
```
  * 第二个参数回调函数cb（newVal, oldVal）
  * 第三个参数一个options配置项是一个对象，可不传 undefined
```
{
  immediate: true, // 是否立即调用一次
  deep: true // 是否开启深度监听， 如果是reactive类型，是否设置此属性默认深度监听
}
```

#### watchEffect高级侦听器）
* 立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数
* 如果用到message 就只会监听message 就是用到几个监听几个 而且是非惰性 会默认调用一次
* 清除副作用
  * 就是在触发监听之前会调用一个函数可以处理你的逻辑例如防抖
* 停止跟踪 watchEffect 返回一个函数 调用之后将停止更新

* 更多的配置项
  * 副作用刷新时机 flush 一般使用post(可以获取到DOM元素)
  * 更新时机，pre组件更新前执行， sync强制效果始终同步触发， post组件更新后执行
  * onTrigger  可以帮助我们调试 watchEffect
```
<div>
  <input id="inp" v-model="message" type="text" />
  <input v-model="message2" type="text" />
  <button @click="stopWatch">停止监听</button>
</div>

import { ref, watch, watchEffect, reactive } from 'vue';
const message = ref<string>('凤');
const message2 = ref<string>('凤凤');
// 默认会执行一次 
const stop = watchEffect((oninvalidate) => {
  const inp:HTMLInputElement = document.querySelector('#inp') as HTMLInputElement;
  console.log('message--------', message.value);
  // console.log('message2--------', message2.value);
  console.log(inp, '进入页面');
  oninvalidate(() => {
    console.log('before');
  })
}, {
  flush: 'post',
  onTrigger(e) {
    // dev可以帮助我们调试 watchEffect
  }
})

const stopWatch = () => stop();
```

#### 识组件&Vue3生命周期
* 每一个.vue 文件都可以充当组件来使用
* 每一个组件都可以复用
* 父组件使用
  * 引入子组件 xxx 然后直接就可以去当标签去使用 （切记组件名称不能与html元素标签名称一样）
  * vue3使用组件无需注册

* 组件的生命周期
  * 是一个组件从创建到销毁的过程成为生命周期
  * 在我们使用Vue3 组合式API 是没有 beforeCreate 和 created 这两个生命周期的
* onBeforeMount()
  * 在组件DOM实际渲染安装之前调用。在这一步中，根元素还不存在
* onMounted()
  * 在组件的第一次渲染后调用，该元素现在可用，允许直接DOM访问
* onBeforeUpdate()
  * 数据更新时调用，发生在虚拟 DOM 打补丁之前
* updated()
  * DOM更新后，updated的方法即会调用
* *nBeforeUnmounted()
  * 在卸载组件实例之前调用。在这个阶段，实例仍然是完全正常的
* onUnmounted()
  * 卸载组件实例后调用。调用此钩子时，组件实例的所有指令都被解除绑定，所有事件侦听器都被移除，所有子组件实例被卸载

#### 实操组件和认识less 和 scoped
* less
  * Less （Leaner Style Sheets 的缩写） 是一门向后兼容的 CSS 扩展语言。这里呈现的是 Less 的官方文档（中文版），包含了 Less 语言以及利用 JavaScript 开发的用于将 Less 样式转换成 CSS 样式的 Less.js 工具
  * Less 和 CSS 非常像，很容易学习。而且 Less 仅对 CSS 语言增加了少许方便的扩展，这就是 Less 如此易学的原因之一 
    * <a href="https://zhuanlan.zhihu.com/p/400573436" style="color: blue;">官方文档 Less 快速入门 | Less.js 中文文档 - Less 中文网</a>
  * 在vite中使用less
```
npm install less less-loader -D // 安装即可
```
* 在style标签注明即可
```
<style lang="less">
 
</style>
```
*  scoped
  * 实现组件的私有化, 当前style属性只属于当前模块
    * 在DOM结构中可以发现,vue通过在DOM结构以及css样式上加了唯一标记,达到样式私有化,不污染全局的作用
    * 样式穿透问题学到第三方组件精讲 ::v-deep  >>> /deep/

#### 父子组件传参
* 父组件通过v-bind绑定一个数据，然后子组件通过defineProps接受传过来的值
  * 传递了一个title 字符串类型是不需要v-bind
  * 传递非字符串类型需要加v-bind  简写 冒号
* 子组件接受值
  * 通过defineProps 来接受 defineProps是无须引入的直接使用即可
  * 如果我们使用的TypeScript，可以使用传递字面量类型的纯类型语法做为参数
* TS 特有的默认值方式
* withDefaults是个函数也是无须引入开箱即用接受一个props函数第二个参数是一个对象设置默认值

* 子组件给父组件传参
  * 是通过defineEmits派发一个事件
```
<template>
    <div class="menu">
        <button @click="clickTap">派发给父组件</button>
    </div>
</template>
 
<script setup lang="ts">
import { reactive } from 'vue'
const list = reactive<number[]>([4, 5, 6])
 
const emit = defineEmits(['on-click'])
const clickTap = () => {
    emit('on-click', list)
}
</script>
```
* 在子组件绑定了一个click 事件 然后通过defineEmits 注册了一个自定义事件
  * 点击click 触发 emit 去调用我们注册的事件 然后传递参数
  * 父组件接受子组件的事件
* 组件暴露给父组件内部属性
  * 通过defineExpose
  * 父组件获取子组件实例通过ref
  * 父组件想要读到子组件的属性可以通过 defineExpose暴露

#### 动态组件
* 让多个组件使用同一个挂载点，并动态切换，这就是动态组件。
* 在挂载点使用component标签，然后使用v-bind:is=”组件”
```
  <component :is="A"></component>
```
* 使用场景
  * tab切换 居多
* 注意事项 
1.在Vue2 的时候is 是通过组件名称切换的 在Vue3 setup 是通过组件实例切换的

2.如果你把组件实例放到Reactive Vue会给你一个警告runtime-core.esm-bundler.js:38 [Vue warn]: Vue received a Component which was made a reactive object. This can lead to unnecessary performance overhead, and should be avoided by marking the component with `markRaw` or using `shallowRef` instead of `ref`. 
Component that was made reactive: 
* 这是因为reactive 会进行proxy 代理 而我们组件代理之后毫无用处 节省性能开销 推荐我们使用shallowRef 或者  markRaw 跳过proxy 代理
```
const tab = reactive<Com[]>([{
    name: "A组件",
    comName: markRaw(A)
}, {
    name: "B组件",
    comName: markRaw(B)
}])
```
* ?.length的语法
  * 某个对象下的某个数组不存在，读取length会报错，处理方式(是否存在，不存在并赋值为空数组) item.children?.length ?? []
  * null ?? [] 得到[],但是 0 ?? [] 得到 0 false ?? [] 得到false

* vue2 与 vue3使用方式区别
* <component :is="current.comName"></component>
* vue3中 is 如果是字符串，不会被渲染  setup 与 component之间没有建立媒介关系
* 改写为vue 2方式没有问题
```
<script lang="ts">
export default {
  components: {
    A,
  }
}
</script>
``` 

