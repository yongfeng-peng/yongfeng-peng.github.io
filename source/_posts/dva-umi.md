---
title: dva+umi
date: 2021-01-14 14:28:46
categories:
- 前端框架
tags:
---

### dva+umi理解
#### [dva](https://dvajs.com/)
* [why dva？](https://github.com/dvajs/dva/issues/1)
* redux数据流的控制可以让应用更控，让逻辑更清晰
* redux同时会有的疑问？概念太多，并且reducer、saga、action都是分离的（文件分离
  * 编辑成本高，需要在reducer、saga、action之间来回切换
  * 不便于组织义务模型（domain models），如：每个模块功能，需要复制很多文件
  * saga书写复杂，每监听一个action都要走fork -> watcher -> worker流程
  * entry书写麻烦
* [what's dva?](https://github.com/dvajs/dva/issues/1)
  * dva是基于现有应用框架(redux + redux-saga + react-router等)的轻量封装，没有引入任何新的概念
  * dva是framework，不是library，类似emberjs，会明确的告诉你每个部件应该怎么写，对于团队而言，会更可控。除了react和rect-dom是peerDependencies以外，dva封装了所有其它依赖
  * dva实现上尽量不创建新语法，而是依赖库本身的语法
  * dva的核心是提供了app.model方法，用于把reducer、initialState、saga、action封装到一起
  ```
  app.model({
    namespace: 'products',
    state: {
      list: [],
      loading: false,
    },
    subscriptions: [
      function(dispatch) {
        dispatch({type: 'products/query'});
      },
    ],
    effects: {
      ['products/query']: function*() {
        yield call(delay(800));
        yield put({
          type: 'products/query/success',
          payload: ['ant-tool', 'roof'],
        });
      },
    },
    reducers: {
      ['products/query'](state) {
        return { ...state, loading: true, };
      },
      ['products/query/success'](state, { payload }) {
        return { ...state, loading: false, list: payload };
      },
    },
  });
  ```
  * 维护简单、提高开发效率
  * 对原有知识点组合，对API对使用

#### [umi](https://umijs.org/zh-CN/docs)
* dva+umi关系
![Image text](https://raw.githubusercontent.com/yongfeng-peng/yongfeng-peng.github.io/dev/source/images/dva.png)
* [dva+umi数据共享](https://github.com/yongfeng-peng/dva-umi-ts-antd.git)
![Image text](https://raw.githubusercontent.com/yongfeng-peng/yongfeng-peng.github.io/dev/source/images/dva-lc.png)
