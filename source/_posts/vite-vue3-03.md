---
title: vite-vue3-03
date: 2022-03-21 16:33:58
categories:
- 前端框架
tags:
---

### vite
#### 依赖注入Provide / Inject
* Provide / Inject
  * 通常，当我们需要我们需要从父组件向子组件传递数据时，我们使用 <a href="https://v3.cn.vuejs.org/guide/component-props.html" style="color: blue;"> props</a>。想象一下这样的结构：有一些深度嵌套的组件，而深层的子组件只需要父组件的部分内容。在这种情况下，如果仍然将 prop 沿着组件链逐级传递下去，可能会很麻烦。
  * provide 可以在祖先组件中指定我们想要提供给后代组件的数据或方法，而在任何后代组件中，我们都可以使用 inject 来接收 provide 提供的数据或方法。 
*  <span style="color: red;">TIPS 你如果传递普通的值 是不具有响应式的 需要通过ref reactive 添加响应式</span> 
* 使用场景
  * 当父组件有很多数据需要分发给其子代组件的时候， 就可以使用provide和inject。
* 父组件传递数据
```
<template>
    <div class="App">
        <button>我是App</button>
        <A></A>
    </div>
</template>
    
<script setup lang='ts'>
import { provide, ref } from 'vue'
import A from './components/A.vue'
let flag = ref<number>(1)
provide('flag', flag)
</script>
    
<style>
.App {
    background: blue;
    color: #fff;
}
</style>
```
* 子组件接受
```
<template>
    <div style="background-color: green;">
        我是B
        <button @click="change">change falg</button>
        <div>{{ flag }}</div>
    </div>
</template>
    
<script setup lang='ts'>
import { inject, Ref, ref } from 'vue'
 
const flag = inject<Ref<number>>('flag', ref(1))
 
const change = () => {
    flag.value = 2
}
</script>
    
<style>
</style>
```

#### 兄弟组件传参和Bus
* 借助父组件传参
  * 父组件为App 子组件为A 和 B他两个是同级的
```
<template>
    <div>
        <A @on-click="getFalg"></A>
        <B :flag="Flag"></B>
    </div>
</template>
    
<script setup lang='ts'>
import A from './components/A.vue'
import B from './components/B.vue'
import { ref } from 'vue'
let Flag = ref<boolean>(false)
const getFalg = (flag: boolean) => {
   Flag.value = flag;
}
</script>
    
<style>
</style>
```
* A 组件派发事件通过App.vue 接受A组件派发的事件然后在Props 传给B组件 也是可以实现的
* 缺点就是比较麻烦 ，无法直接通信，只能充当桥梁

* Event Bus
  * 我们在Vue2 可以使用$emit 传递 $on监听 emit传递过来的事件
  * 这个原理其实是运用了JS设计模式之发布订阅模式
```
type BusClass<T> = {
    emit: (name: T) => void
    on: (name: T, callback: Function) => void
}
type BusParams = string | number | symbol 
type List = {
    [key: BusParams]: Array<Function>
}
class Bus<T extends BusParams> implements BusClass<T> {
    list: List
    constructor() {
        this.list = {}
    }
    emit(name: T, ...args: Array<any>) {
        let eventName: Array<Function> = this.list[name]
        eventName.forEach(ev => {
            ev.apply(this, args)
        })
    }
    on(name: T, callback: Function) {
        let fn: Array<Function> = this.list[name] || [];
        fn.push(callback)
        this.list[name] = fn
    }
}
 
export default new Bus<number>()
```
* 挂载到Vue config 全局就可以使用啦

#### TSX
* 使用Template去写我们模板。现在可以扩展另一种风格TSX风格
* vue2 的时候就已经支持jsx写法，只不过不是很友好，随着vue3对typescript的支持度，tsx写法越来越被接受
* npm install @vitejs/plugin-vue-jsx -D
* vite.config.ts 配置
```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'; // 引入jsx
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),vueJsx()] // 注册vueJsx
})
```
* 修改tsconfig.json 配置文件
```
"jsx": "preserve",
"jsxFactory": "h",
"jsxFragmentFactory": "Fragment",
```
* 使用TSX
* <span style="color: red;">TIPS tsx不会自动解包使用ref加.vlaue ! ! !</span> 
* tsx支持 v-model 的使用

```
import { ref } from 'vue'
let v = ref<string>('')
const renderDom = () => {
  return (
    <>
      <input v-model={v.value} type="text" />
      <div>
          {v.value}
      </div>
    </>
  )
}
export default renderDom
```
* v-show
```
import { ref } from 'vue'
let flag = ref(false)
const renderDom = () => {
  return (
    <>
      <div v-show={flag.value}>景天</div>
      <div v-show={!flag.value}>雪见</div>
    </>
  )
}
export default renderDom
```
* v-if是不支持的(需要修改编写)
```
import { ref } from 'vue'
let flag = ref(false)
const renderDom = () => {
  return (
    <>
      {
        flag.value ? <div>景天</div> : <div>雪见</div>
      }
    </>
  )
}
export default renderDom
```
* v-for也是不支持的(需要使用Map)
```
import { ref } from 'vue'
let arr = [1,2,3,4,5]
const renderDom = () => {
  return (
    <>
      {
        arr.map(v=>{
          return <div>${v}</div>
        })
      }
    </>
  )
}
export default renderDom
```
* v-bind使用(直接赋值就可以)
```
import { ref } from 'vue'
let arr = [1, 2, 3, 4, 5]
const renderDom = () => {
  return (
    <>
      <div data-arr={arr}>1</div>
    </>
  )
}
export default renderDom
```
* v-on绑定事件 所有的事件都按照react风格来
  * 所有事件有on开头
  * 所有事件名称首字母大写
```
const renderDom = () => {
  return (
    <>
      <button onClick={clickTap}>点击</button>
    </>
  )
}
const clickTap = () => {
  console.log('click');
}
export default renderDom
```
* Props 接受值
```
import { ref } from 'vue'
type Props = {
    title:string
}
const renderDom = (props:Props) => {
  return (
    <>
      <div>{props.title}</div>
      <button onClick={clickTap}>点击</button>
    </>
  )
}
const clickTap = () => {
  console.log('click');
}
export default renderDom
```
* Emit派发
```
type Props = {
  title: string
}
const renderDom = (props: Props,content:any) => {
  return (
    <>
      <div>{props.title}</div>
      <button onClick={clickTap.bind(this,content)}>点击</button>
    </>
  )
}
const clickTap = (ctx:any) => {
  ctx.emit('on-click',1)
}
```

#### 深入v-model
* Vue3自动引入插件
  * unplugin-auto-import/vite
* vite配置
```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import VueJsx from '@vitejs/plugin-vue-jsx'
import AutoImport from 'unplugin-auto-import/vite'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),VueJsx(),AutoImport({
    imports:['vue'],
    dts:"src/auto-import.d.ts"
  })]
})
```
* 配置完成之后使用ref reactive watch 等 无须import 导入 可以直接使用 
  * <a href="https://github.com/antfu/unplugin-auto-import" style="color: blue;"> v-model</a>
* v-model
*  <span style="color: red;">TIps 在Vue3 v-model 是破坏性更新的</span> 
* v-model在组件里面也是很重要的
  * v-model 其实是一个语法糖 通过props 和 emit组合而成的
  * 默认值的改变
  * prop：value -> modelValue；
  * 事件：input -> update:modelValue；
  * v-bind 的 .sync 修饰符和组件的 model 选项已移除
  * 新增 支持多个v-model
  * 新增 支持自定义 修饰符
* 子组件
```
<template>
     <div v-if='propData.modelValue ' class="dialog">
         <div class="dialog-header">
             <div>标题</div><div @click="close">x</div>
         </div>
         <div class="dialog-content">
            内容
         </div>
         
     </div>
</template>
 
<script setup lang='ts'>
 
type Props = {
   modelValue:boolean
}
 
const propData = defineProps<Props>()
 
const emit = defineEmits(['update:modelValue'])
 
const close = () => {
     emit('update:modelValue',false)
}
 
</script>
 
<style lang='less'>
.dialog{
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;
    position: fixed;
    left:50%;
    top:50%;
    transform: translate(-50%,-50%);
    &-header{
        border-bottom: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        padding: 10px;
    }
    &-content{
        padding: 10px;
    }
}
</style>
```
* 父组件
```
<template>
  <button @click="show = !show">开关{{show}}</button>
  <Dialog v-model="show"></Dialog>
</template>
 
<script setup lang='ts'>
import Dialog from "./components/Dialog/index.vue";
import {ref} from 'vue'
const show = ref(false)
</script>
 
<style>
</style>
```
* 绑定多个案例
* 子组件
```
<template>
     <div v-if='modelValue ' class="dialog">
         <div class="dialog-header">
             <div>标题---{{title}}</div><div @click="close">x</div>
         </div>
         <div class="dialog-content">
            内容
         </div>
         
     </div>
</template>
 
<script setup lang='ts'>
 
type Props = {
   modelValue:boolean,
   title:string
}
 
const propData = defineProps<Props>()
 
const emit = defineEmits(['update:modelValue','update:title'])
 
const close = () => {
     emit('update:modelValue',false)
     emit('update:title','我要改变')
}
 
</script>
 
<style lang='less'>
.dialog{
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;
    position: fixed;
    left:50%;
    top:50%;
    transform: translate(-50%,-50%);
    &-header{
        border-bottom: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        padding: 10px;
    }
    &-content{
        padding: 10px;
    }
}
</style>
```
* 父组件
```
<template>
  <button @click="show = !show">开关{{show}} ----- {{title}}</button>
  <Dialog v-model:title='title' v-model="show"></Dialog>
</template>
 
<script setup lang='ts'>
import Dialog from "./components/Dialog/index.vue";
import {ref} from 'vue'
const show = ref(false)
const title = ref('我是标题')
</script>
 
<style>
</style>
```
* 自定义修饰符
  * 添加到组件 v-model 的修饰符将通过 modelModifiers prop 提供给组件。在下面的示例中，我们创建了一个组件，其中包含默认为空对象的 modelModifiers prop
```
<script setup lang='ts'>
type Props = {
  modelValue: boolean,
  title?: string,
  modelModifiers?: {
    default: () => {}
  }
  titleModifiers?: {
    default: () => {}
  }
}
const propData = defineProps<Props>()
const emit = defineEmits(['update:modelValue', 'update:title'])
const close = () => {
  console.log(propData.modelModifiers);
  emit('update:modelValue', false)
  emit('update:title', '我要改变')
}
```

#### 自定义指令directive
* directive-自定义指令（属于破坏性更新）
  * Vue中有v-if,v-for,v-bind，v-show,v-model 等等一系列方便快捷的指令 今天一起来了解一下vue里提供的自定义指令
* Vue3指令的钩子函数
  * created 元素初始化的时候
  * beforeMount 指令绑定到元素后调用 只调用一次
  * mounted 元素插入父级dom调用
  * beforeUpdate 元素被更新之前调用
  * update 这个周期方法被移除 改用updated
  * beforeUnmount 在元素被移除前调用
  * unmounted 指令被移除后调用 只调用一次
  * Vue2 指令 bind inserted update componentUpdated unbind
* 在setup内定义局部指令
  * 但这里有一个需要注意的限制：必须以 vNameOfDirective 的形式来命名本地自定义指令，以使得它们可以直接在模板中使用
```
<template>
  <button @click="show = !show">开关{{show}} ----- {{title}}</button>
  <Dialog  v-move-directive="{background:'green',flag:show}"></Dialog>
</template>
 
const vMoveDirective: Directive = {
  created: () => {
    console.log("初始化====>");
  },
  beforeMount(...args: Array<any>) {
    // 在元素上做些操作
    console.log("初始化一次=======>");
  },
  mounted(el: any, dir: DirectiveBinding<Value>) {
    el.style.background = dir.value.background;
    console.log("初始化========>");
  },
  beforeUpdate() {
    console.log("更新之前");
  },
  updated() {
    console.log("更新结束");
  },
  beforeUnmount(...args: Array<any>) {
    console.log(args);
    console.log("======>卸载之前");
  },
  unmounted(...args: Array<any>) {
    console.log(args);
    console.log("======>卸载完成");
  },
};
```
* 生命周期钩子参数详解
  * 第一个 el  当前绑定的DOM 元素
  * 第二个 binding
    * instance：使用指令的组件实例。
    * value：传递给指令的值。例如，在 v-my-directive="1 + 1" 中，该值为 2。
    * oldValue：先前的值，仅在 beforeUpdate 和 updated 中可用。无论值是否有更改都可用。
    * arg：传递给指令的参数(如果有的话)。例如在 v-my-directive:foo 中，arg 为 "foo"。
    * modifiers：包含修饰符(如果有的话) 的对象。例如在 v-my-directive.foo.bar 中，修饰符对象为 {foo: true，bar: true}。
    * dir：一个对象，在注册指令时作为参数传递。例如，在以下指令中
* 第三个 当前元素的虚拟DOM 也就是Vnode
* 第四个 prevNode 上一个虚拟节点，仅在 beforeUpdate 和 updated 钩子中可用 
* 函数简写
  * 你可能想在 mounted 和 updated 时触发相同行为，而不关心其他的钩子函数。那么你可以通过将这个函数模式实现
```
<template>
   <div>
      <input v-model="value" type="text" />
      <A v-move="{ background: value }"></A>
   </div>
</template>
   
<script setup lang='ts'>
import A from './components/A.vue'
import { ref, Directive, DirectiveBinding } from 'vue'
let value = ref<string>('')
type Dir = {
   background: string
}
const vMove: Directive = (el, binding: DirectiveBinding<Dir>) => {
   el.style.background = binding.value.background
}
</script>
 
<style>
</style>
```
* 自定义拖拽指令
```
<template>
  <div v-move class="box">
    <div class="header"></div>
    <div>
      内容
    </div>
  </div>
</template>
 
<script setup lang='ts'>
import { Directive } from "vue";
const vMove: Directive = {
  mounted(el: HTMLElement) {
    let moveEl = el.firstElementChild as HTMLElement;
    const mouseDown = (e: MouseEvent) => {
      //鼠标点击物体那一刻相对于物体左侧边框的距离=点击时的位置相对于浏览器最左边的距离-物体左边框相对于浏览器最左边的距离
      console.log(e.clientX, e.clientY, "-----起始", el.offsetLeft);
      let X = e.clientX - el.offsetLeft;
      let Y = e.clientY - el.offsetTop;
      const move = (e: MouseEvent) => {
        el.style.left = e.clientX - X + "px";
        el.style.top = e.clientY - Y + "px";
        console.log(e.clientX, e.clientY, "---改变");
      };
      document.addEventListener("mousemove", move);
      document.addEventListener("mouseup", () => {
        document.removeEventListener("mousemove", move);
      });
    };
    moveEl.addEventListener("mousedown", mouseDown);
  },
};
</script>
 
<style lang='less'>
.box {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 200px;
  border: 1px solid #ccc;
  .header {
    height: 20px;
    background: black;
    cursor: move;
  }
}
</style>
```
#### 自定义Hooks
* Vue3 自定义Hook
  * 主要用来处理复用代码逻辑的一些封装
    * 这个在vue2 就已经有一个东西是Mixins
    * mixins就是将这些多个相同的逻辑抽离出来，各个组件只需要引入mixins，就能实现一次写代码，多组件受益的效果。
    * 弊端就是 会涉及到覆盖的问题
    * 组件的data、methods、filters会覆盖mixins里的同名data、methods、filters
* 二点就是 变量来源不明确（隐式传入），不利于阅读，使代码变得难以维护。
  * Vue3 的自定义的hook
  * Vue3 的 hook函数 相当于 vue2 的 mixin, 不同在与 hooks 是函数
  * Vue3 的 hook函数 可以帮助我们提高代码的复用性, 让我们能在不同的组件中都利用 hooks 函数
  * Vue3 hook <a href="https://github.com/antfu/unplugin-auto-import" style="color: blue;">库Get Started | VueUse</a>
```
import { onMounted } from 'vue'
type Options = {
    el: string
}
 
type Return = {
    Baseurl: string | null
}
export default function (option: Options): Promise<Return> {
    return new Promise((resolve) => {
        onMounted(() => {
            const file: HTMLImageElement = document.querySelector(option.el) as HTMLImageElement;
            file.onload = ():void => {
                resolve({
                    Baseurl: toBase64(file)
                })
            }
 
        })
        const toBase64 = (el: HTMLImageElement): string => {
            const canvas: HTMLCanvasElement = document.createElement('canvas')
            const ctx = canvas.getContext('2d') as CanvasRenderingContext2D
            canvas.width = el.width
            canvas.height = el.height
            ctx.drawImage(el, 0, 0, canvas.width,canvas.height)
            console.log(el.width);
            
            return canvas.toDataURL('image/png')
 
        }
    })
}
```

####  Vue3定义全局函数和变量
* globalProperties
  * 由于Vue3 没有Prototype 属性 使用 app.config.globalProperties 代替 然后去定义变量和函数
* Vue2
```
// 之前 (Vue 2.x)
Vue.prototype.$http = () => {}
```
* Vue3
```
// 之后 (Vue 3.x)
const app = createApp({})
app.config.globalProperties.$http = () => {}
```
* 过滤器
  * 在Vue3 移除了
  * 正好可以使用全局函数代替Filters
```
app.config.globalProperties.$filters = {
  format<T extends any>(str: T): string {
    return `$${str}`
  }
}
```
* 声明文件 不然TS无法正确类型 推导
```
type Filter = {
    format: <T extends any>(str: T) => T
  }
// 声明要扩充@vue/runtime-core包的声明.
// 这里扩充"ComponentCustomProperties"接口, 因为他是vue3中实例的属性的类型.
  declare module '@vue/runtime-core' {
    export interface ComponentCustomProperties {
        $filters: Filter
    }
  }
```
* setup 读取值
```
import { getCurrentInstance, ComponentInternalInstance } from 'vue';
const { appContext } = <ComponentInternalInstance>getCurrentInstance()
console.log(appContext.config.globalProperties.$env);
```

#### 编写Vue3插件
* 插件
  * 插件是自包含的代码，通常向 Vue 添加全局级功能。你如果是一个对象需要有install方法Vue会帮你自动注入到install 方法 你如果是function 就直接当install 方法去使用
* 使用插件
  * 在使用 createApp() 初始化 Vue 应用程序后，你可以通过调用 use() 方法将插件添加到你的应用程序中。
* 实现一个Loading
```
<template>
    <div v-if="isShow" class="loading">
        <div class="loading-content">Loading...</div>
    </div>
</template>
    
<script setup lang='ts'>
import { ref } from 'vue';
const isShow = ref(false)//定位loading 的开关
 
const show = () => {
    isShow.value = true
}
const hide = () => {
    isShow.value = false
}
//对外暴露 当前组件的属性和方法
defineExpose({
    isShow,
    show,
    hide
})
</script>

<style scoped lang="less">
.loading {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    &-content {
        font-size: 30px;
        color: #fff;
    }
}
</style>
```
* Loading.ts
```
import {  createVNode, render, VNode, App } from 'vue';
import Loading from './index.vue'
export default {
  install(app: App) {
    //createVNode vue提供的底层方法 可以给我们组件创建一个虚拟DOM 也就是Vnode
    const vnode: VNode = createVNode(Loading)
    //render 把我们的Vnode 生成真实DOM 并且挂载到指定节点
    render(vnode, document.body)
    // Vue 提供的全局配置 可以自定义
    app.config.globalProperties.$loading = {
        show: () => vnode.component?.exposed?.show(),
        hide: () => vnode.component?.exposed?.hide()
    }
  }
}
```
* Main.ts
```
import Loading from './components/loading'
let app = createApp(App)
app.use(Loading)
type Lod = {
    show: () => void,
    hide: () => void
}
//编写ts loading 声明文件放置报错 和 智能提示
declare module '@vue/runtime-core' {
    export interface ComponentCustomProperties {
        $loading: Lod
    }
}
app.mount('#app')
```

#### 了解UI库ElementUI，AntDesigin等
* vue作为一款深受广大群众以及尤大崇拜者的喜欢，特此列出在github上开源的vue优秀的UI组件库供大家参考
  * 这两套框架主要用于后台管理系统的制作，方便开发者快速开发
* 安装方法
```
# NPM
$ npm install element-plus --save
 
# Yarn
$ yarn add element-plus
 
# pnpm
$ pnpm install element-plus
```
* main ts引入
```
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'
 
const app = createApp(App)
 
app.use(ElementPlus)
app.mount('#app')
```
* volar插件支持
```
{
  "compilerOptions": {
    // ...
    "types": ["element-plus/global"]
  }
}
```
* Element UI Plus
  * <a href="https://element-plus.gitee.io/zh-CN/" style="color: blue;">一个 Vue 3 UI 框架 | Element Plus</a>
* Ant Design Vue
  * <a href=" https://next.antdv.com/docs/vue/introduce-cn" style="color: blue;">Ant Design Vue</a>
  * 安装
  ```
  $ npm install ant-design-vue@next --save
  $ yarn add ant-design-vue@next
```
  * 使用
```
import { createApp } from 'vue';
import Antd from 'ant-design-vue';
import App from './App';
import 'ant-design-vue/dist/antd.css';
 
const app = createApp(App);
 
app.use(Antd).mount('#app');
```

#### 详解Scoped和样式 穿透
* 主要是用于修改很多vue常用的组件库（element, vant, AntDesigin），虽然配好了样式但是还是需要更改其他的样式
* 就需要用到样式穿透
* scoped的原理
  * vue中的scoped 通过在DOM结构以及css样式上加唯一不重复的标记:data-v-hash的方式，以保证唯一（而这个工作是由过PostCSS转译实现的），达到样式私有化模块化的目的。
* 总结一下scoped三条渲染规则：
  * 给HTML的DOM节点加一个不重复data属性(形如：data-v-123)来表示他的唯一性
  * 在每句css选择器的末尾（编译后的生成的css语句）加一个当前组件的data属性选择器（如[data-v-123]）来私有化样式
  * 如果组件内部包含有其他组件，只会给其他组件的最外层标签加上当前组件的data属性
* PostCSS会给一个组件中的所有dom添加了一个独一无二的动态属性data-v-xxxx，然后，给CSS选择器额外添加一个对应的属性选择器来选择该组件中dom，这种做法使得样式只作用于含有该属性的dom——组件内部dom, 从而达到了'样式模块化'的效果.
* 案例修改Element ui Input样式
  * 如果不写Scoped 就没问题
  * 原因就是Scoped 搞的鬼 他在进行PostCss转化的时候把元素选择器默认放在了最后
  * Vue 提供了样式穿透:deep() 他的作用就是用来改变 属性选择器的位置
