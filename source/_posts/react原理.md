---
title: react原理
date: 2020-12-23 14:25:09
categories:
- 前端框架
tags:
---

### react原理剖析
#### [核心API](https://github.com/facebook/react/blob/master/packages/react/src/React.js)
  * React.createElement：创建虚拟的DOM
  * React.Component: 实现自定义组件
  * ReactDOM.render: 渲染真实DOM
  
* [react-dom](https://github.com/facebook/react/blob/master/packages/react-dom/src/client/ReactDOM.js)：主要是render逻辑

#### JSX是什么
  * [在线JSX预编译](https://reactjs.org/)
  * react使用JSX来替代常规的JavaScript，JSX是一个看起来很像XML的JavaScript语法扩展
#### 为什么需要JXS
  * JSX执行更快，因为它在编译为JavaScript代码后进行了优化（在react里面可以用它描述视图）
  * 它是**类型安全**的，在编译过程中就能发现错误（编译器可以对它进行规范处理、一系列严谨转换、类型检测）
  * 使用JSX编写模版更加简单快捷（开发效率）
#### JXS怎么用
  * 原理：babel-loader会预编译JSX为React.createElement(...)

#### setState
  * class组件的特点，就是拥有特殊状态并且可以通过setState更新状态，从而重新渲染视图，是学习React中最重要的api
  * setState并没有直接操作去渲染，而是执行了一个异步的updater队列,使用一个类来专门管理

#### 虚拟dom
* 用JavaScript对象表示DOM信息和结构，当状态变更的时候，重新渲染这个JavaScript的对象结构。这个JavaScript对象称为virtual dom
* 简单的说虚拟dom就是js对象，可以描述dom

#### diff算法
* diff策略
  * 同级比较，Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计
  * 拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构。例如：div->p, CompA->CompB
  * 对于同一层级的一组子节点，通过唯一的key进行区分
* 基于以上三个前提策略，React 分别对 tree diff、component diff 以及 element diff 进行算法优化，事实也证明，这三个前提策略是合理且准确的，它保证了整体界面构建的性能
* element diff 差异类型：
  * 替换原来的节点，例如把div换成了p，Comp1换成Comp2
  * 移动、删除、新增子节点， 例如ul中的多个子节点li中出现了顺序互换
  * 修改了节点的属性，例如节点类名发生了变化
  * 对于文本节点，文本内容可能会改变
* 重排（reorder）操作：INSERT_MARKUP（插入）、MOVE_EXISTING（移动）和 REMOVE_NODE（删除）
  * INSERT_MARKUP，新的 component 类型不在老集合里， 即是全新的节点，需要对新节点执行插入操作
  * MOVE_EXISTING，在老集合有新 component 类型，且 element 是可更新的类型，generateComponentChildren 已调用 receiveComponent，这种情况下 prevChild=nextChild，就需要做移动操作，可以复用以前的   DOM 节点
  * REMOVE_NODE，老 component 类型，在新集合里也有，但对应的 element 不同则不能直接复用和更新，需要执行删除操作，或者老 component 不在新集合里的，也需要执行删除操作
* 为什么需要虚拟dom和对diff算法理解
  * 抽象出虚拟dom层，减少重布局、重排、重绘次数，不直接更新真实dom
  * 比较出应不应该更新dom，即使更新，尽可能的一次性、批量的更新