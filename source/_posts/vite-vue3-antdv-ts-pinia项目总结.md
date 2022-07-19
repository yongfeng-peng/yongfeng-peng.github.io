<!--
 * @Author: yfp
 * @Date: 2022-07-19 09:50:09
 * @LastEditors: yfp
 * @LastEditTime: 2022-07-19 16:38:06
 * @Description: 
-->
---
title: vite（2.9.9）+vue3+antdv（3.2.3）+ts+pinia项目总结
date: 2022-07-19 09:50:09
categories:
- 前端框架
tags:
---

### 总结
#### vite项目require语法兼容问题
* 使用插件，vite.config.ts配置
  * yarn add -D vite-plugin-require-transform
  * npm i vite-plugin-require-transform --save-dev
* 遇到问题 build不成功，与ant-vue组件冲突
* 解决 <a href="https://vitejs.cn/guide/assets.html#the-public-directory" style="color: blue;">vite静态资源处理</a>

#### 使用vite-plugin-html插件，vite.config.ts配置；拦截接口，重定向其它内网地址获取登录信息，无效
  * yarn add vite-plugin-html -D
  * npm i vite-plugin-html -D

#### 父子组件的传参无效
  * 父组件定义参数 let info = reactive({})，子组件接收不到
  * 解决 该用ref定义，reactive定义需要解构，一个个传递

#### 子组件更新父组件传递的<a-modal>状态
```
const emit = defineEmits(["update:show"]);
emit("update:show", false);
```

#### <a-form>校验，滚动到第一个校验失败位置
```
// 由于校验失败ant会自动给失败表单项添加类名，直接获取滚动到对应位置
setTimeout(() => {
  const errorList = (document as any).querySelectorAll(".ant-form-item-has-error");
  if (errorList.length) {
    errorList[0].scrollIntoView({
      block: "center",
      behavior: "smooth",
    });
    return;
  }
}, 50);
```

#### <a-form>校验,手动触发校验及清除校验
```
// ref="formRef"
const formRef = ref<FormInstance>();
(formRef.value as any)
  .validateFields()
  .then(() => {
    // 校验成功，逻辑
  })
  .catch((errorInfo: any) => {});

// rpName 清除校验字段
formRef.value as any).clearValidate(["rpName"]);
```

#### lodash使用
```
// import { throttle } from "lodash-es";
const throttleFun = throttle(
  () => {
    initClassTree();
  },
  500,
  {
    leading: true,
    trailing: true,
  }
);
```


