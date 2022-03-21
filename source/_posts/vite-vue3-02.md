---
title: vite-vue3-02
date: 2022-03-18 16:20:54
categories:
- 前端框架
tags:
---

### vite
#### 插槽slot(占位符)
* 子组件中的提供给父组件使用的一个占位符，用<slot></slot> 表示，父组件可以在这个占位符中填充任何模板代码，如 HTML、组件等，填充的内容会替换子组件的<slot></slot>标签
* 匿名插槽
* 在子组件放置一个插槽
```
<template>
    <div>
      <slot></slot>
    </div>
</template>
```
* 父组件使用插槽
  * 在父组件给这个插槽填充内容
```
<Dialog>
    <template v-slot>
        <div>2132</div>
    </template>
</Dialog>
```
* 具名插槽
  * 具名插槽其实就是给插槽取个名字。一个子组件可以放多个插槽，而且可以放在不同的地方，而父组件填充内容时，可以根据这个名字把内容填充到对应插槽中
```
<div>
    <slot name="header"></slot>
    <slot></slot>

    <slot name="footer"></slot>
</div>
```
* 父组件使用需对应名称
```
<Dialog>
    <template v-slot:header>
        <div>1</div>
    </template>
    <template v-slot>
        <div>2</div>
    </template>
    <template v-slot:footer>
        <div>3</div>
    </template>
</Dialog>
```
* 插槽简写
```
<Dialog>
    <template #header>
        <div>1</div>
    </template>
    <template #default>
        <div>2</div>
    </template>
    <template #footer>
        <div>3</div>
    </template>
</Dialog>
```
* 作用域插槽
  * 在子组件动态绑定参数 派发给父组件的slot去使用
```
<div>
    <slot name="header"></slot>
    <div>
        <div v-for="item in 100">
            <slot :data="item"></slot>
        </div>
    </div>

    <slot name="footer"></slot>
</div>
```
  * 通过结构方式取值
```
<Dialog>
  <template #header>
      <div>1</div>
  </template>
  <template #default="{ data }">
      <div>{{ data }}</div>
  </template>
  <template #footer>
      <div>3</div>
  </template>
</Dialog>
```
* 动态插槽
  * 插槽可以是一个变量名
```
<Dialog>
    <template #[name]>
        <div>
            23
        </div>
    </template>
</Dialog>
const name = ref('header')
```
#### 异步组件&代码分包&suspense
* 异步组件
* 在大型应用中，我们可能需要将应用分割成小一些的代码块,并且减少主包的体积
* 就可以使用异步组件
* 顶层 await,在setup语法糖里面 使用方法

```
  <script setup> 中可以使用顶层 await 结果代码会被编译成 async setup()
  <script setup>
  const post = await fetch(`/api/post/1`).then(r => r.json())
  </script>
```

* 父组件引用子组件 通过defineAsyncComponent加载异步配合import 函数模式便可以分包
```
<script setup lang="ts">
import { reactive, ref, markRaw, toRaw, defineAsyncComponent } from 'vue'
const Dialog = defineAsyncComponent(() => import('../../components/Dialog/index.vue'))
```
* suspense
  * <suspense> 组件有两个插槽。它们都只接收一个直接子节点。default 插槽里的节点会尽可能展示出来。如果不能，则展示 fallback 插槽里的节点
```
<Suspense>
  <template #default>
    <Dialog>
      <template #default>
          <div>接口请求动态数据</div>
      </template>
    </Dialog>
  </template>

  <template #fallback>
     <div>loading...</div>
  </template>
</Suspense>
```

#### Teleport传送组件
* Teleport Vue 3.0新特性之一
  * Teleport 是一种能够将我们的模板渲染至指定DOM节点，不受父级style、v-show等属性影响，但data、prop数据依旧能够共用的技术；类似于 React 的 Portal
  * 主要解决的问题 因为Teleport节点挂载在其他指定的DOM节点下，完全不受父级style样式影响
  * 使用方法
    * 通过to 属性 插入指定元素位置 to="body" 便可以将Teleport 内容传送到指定位置
```
<Teleport to="body">
  <Loading></Loading>
</Teleport>
```
* 也可以自定义传送位置 支持 class id等 选择器
```
<div id="app"></div>
<div class="modal"></div>

<Teleport to=".modal">
  <Loading></Loading>
</Teleport>
```
* 也可以使用多个
```
<Teleport to=".modal1">
  <Loading></Loading>
</Teleport>
<Teleport to=".modal2">
  <Loading></Loading>
</Teleport>
```

#### keep-alive缓存组件
* 内置组件keep-alive
  * 有时候我们不希望组件被重新渲染影响使用体验；或者处于性能考虑，避免多次重复渲染降低性能。而是希望组件可以缓存下来,维持当前的状态。这时候就需要用到keep-alive组件
* 开启keep-alive 生命周期的变化
  * 初次进入时： onMounted> onActivated
  * 退出后触发 deactivated
  * 再次进入：
  * 只会触发 onActivated
  * 事件挂载的方法等，只执行一次的放在 onMounted中；组件每次进去执行的方法放在 onActivated中
```
<!-- 基本 -->
<keep-alive>
  <component :is="view"></component>
</keep-alive>
 
<!-- 多个条件判断的子组件 -->
<keep-alive>
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>
 
<!-- 和 `<transition>` 一起使用 -->
<transition>
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>
</transition>
```
* include 和 exclude
  * include 和 exclude prop 允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示
```
 <keep-alive :include="" :exclude="" :max=""></keep-alive>
```
* max
```
<keep-alive :max="10">
  <component :is="view"></component>
</keep-alive>
```

####  transition动画组件
* Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡:
  * 条件渲染 (使用 v-if)
  * 条件展示 (使用 v-show)
  *  动态组件
  * 组件根节点
  *  自定义 transition 过度效果，你需要对transition组件的name属性自定义。并在css中写入对应的样式
  * 过渡的类名
    * 在进入/离开的过渡中，会有 6 个 class 切换。
* 过渡 class
  * 在进入/离开的过渡中，会有 6 个 class 切换。
  * v-enter-from：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
  * v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
  * v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时  v-enter-from 被移除)，在过渡/动画完成之后移除。
  * v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
  * v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
  * v-leave-to：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave-from 被移除)，在过渡/动画完成之后移除。
* 自定义过渡 class 类名
  * trasnsition props
  * enter-from-class
  * enter-active-class
  * enter-to-class
  * leave-from-class
  * leave-active-class
  * leave-to-class
  * 自定义过度时间 单位毫秒
    * 你也可以分别指定进入和离开的持续时间：
```
<transition :duration="1000">...</transition>
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```
* 通过自定义class 结合css动画库animate css
* 安装库 npm install animate.css
* 引入 import 'animate.css'
* 使用方法
* <a href="https://animate.style/" style="color: blue;">官方文档 </a>
```
<transition
    leave-active-class="animate__animated animate__bounceInLeft"
    enter-active-class="animate__animated animate__bounceInRight"
>
    <div v-if="flag" class="box"></div>
</transition>
```
* transition 生命周期8个
  * @before-enter="beforeEnter" //对应enter-from
  * @enter="enter"//对应enter-active
  * @after-enter="afterEnter"//对应enter-to
  * @enter-cancelled="enterCancelled"//显示过度打断
  * @before-leave="beforeLeave"//对应leave-from
  * @leave="leave"//对应enter-active
  * @after-leave="afterLeave"//对应leave-to
  * @leave-cancelled="leaveCancelled"//离开过度打断
  * 当只用 JavaScript 过渡的时候，在 enter 和 leave 钩子中必须使用 done 进行回调
  * 结合gsap 动画库使用(npm install gsap -S) 
    * import gsap from 'gsap'
    * <a href="https://greensock.com/" style="color: blue;">GreenSock</a>
```
const beforeEnter = (el: Element) => {
    console.log('进入之前from', el);
}
const Enter = (el: Element,done:Function) => {
    console.log('过度曲线');
    setTimeout(()=>{
       done()
    },3000)
}
const AfterEnter = (el: Element) => {
    console.log('to');
}
```
* appear
  * 通过这个属性可以设置初始节点过度 就是页面加载完成就开始动画 对应三个状态
```
appear-active-class=""
appear-from-class=""
appear-to-class=""
appear
```
#### transition-group过度列表
  * 单个节点
  * 多个节点，每次只渲染一个
  * 那么怎么同时渲染整个列表，比如使用 v-for？在这种场景下，我们会使用 <transition-group> 组件。在我们深入例子之前，先了解关于这个组件的几个特点：
    * 默认情况下，它不会渲染一个包裹元素，但是你可以通过 tag attribute 指定渲染一个元素。
    * 过渡模式不可用，因为我们不再相互切换特有的元素。
    * 内部元素总是需要提供唯一的 key attribute 值。
    * CSS 过渡的类将会应用在内部的元素中，而不是这个组/容器本身。
```
<transition-group>
     <div style="margin: 10px;" :key="item" v-for="item in list">{{ item }</div>
</transition-group>
const list = reactive<number[]>([1, 2, 4, 5, 6, 7, 8, 9])
const Push = () => {
    list.push(123)
}
const Pop = () => {
    list.pop()
}
```
* 列表的移动过渡
  * <transition-group> 组件还有一个特殊之处。除了进入和离开，它还可以为定位的改变添加动画。只需了解新增的 v-move 类就可以使用这个新功能，它会应用在元素改变定位的过程中。像之前的类名一样，它的前缀可以通过 name attribute 来自定义，也可以通过 move-class attribute 手动设置

```
<template>
    <div>
        <button @click="shuffle">Shuffle</button>
        <transition-group class="wraps" name="mmm" tag="ul">
            <li class="cell" v-for="item in items" :key="item.id">{{ item.number }}</li>
        </transition-group>
    </div>
</template>
  
<script setup  lang='ts'>
import _ from 'lodash'
import { ref } from 'vue'
let items = ref(Array.apply(null, { length: 81 } as number[]).map((_, index) => {
    return {
        id: index,
        number: (index % 9) + 1
    }
}))
const shuffle = () => {
    items.value = _.shuffle(items.value)
}
</script>
  
<style scoped lang="less">
.wraps {
    display: flex;
    flex-wrap: wrap;
    width: calc(25px * 10 + 9px);
    .cell {
        width: 25px;
        height: 25px;
        border: 1px solid #ccc;
        list-style-type: none;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
 
.mmm-move {
    transition: transform 0.8s ease;
}
</style>
```
* 状态过渡
  * Vue 也同样可以给数字 Svg 背景颜色等添加过度动画 今天演示数字变化
```
<template>
    <div>
        <input step="20" v-model="num.current" type="number" />
        <div>{{ num.tweenedNumber.toFixed(0) }}</div>
    </div>
</template>
    
<script setup lang='ts'>
import { reactive, watch } from 'vue'
import gsap from 'gsap'
const num = reactive({
    tweenedNumber: 0,
    current:0
})
 
watch(()=>num.current, (newVal) => {
    gsap.to(num, {
        duration: 1,
        tweenedNumber: newVal
    })
})
 
</script>
    
<style>
</style>
```

