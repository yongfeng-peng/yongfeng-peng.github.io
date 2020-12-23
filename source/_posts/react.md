---
title: react
date: 2020-12-19 10:56:17
categories:
- 前端框架
tags:
---

### react
#### React和ReactDOM
* React逻辑控制(义务控制、视图模型控制)，React.createElement()（生成虚拟DOM，React生成的组件或者数据与render渲染的数据进行diff,打补丁）

#### JSX
* 表达式: 
```
{expr}
```
* 属性: 
```
<div id={}></div>
```
* jsx也是表达式:
```
 <p>{jsx}</p>
```

#### 组件
* 函数式：
```
function Comp(props) {
  return(...)
}
```
* 类：
```
class Comp extends React.Component {
  render() {
    return(...)
  }
}
```

#### 属性
```
<Comp name='' style={{...}}>
```

#### 状态
```
class Comp {
  state = {}
  componentDidMount() {
    this.setState({
      prop: val, // 批量异步的，不会立刻生效
    })
    this.setState({state => (prop: val)})
  }
}
```

#### 条件和循环
```
{this.state.isLOgin ? <p>{userInfo.name}</p> : '登陆'}
{this.state.message && <p>{this.state.message}</p>}
{this.state.list.map(u => <li>{u.name}</li>)}
```

#### 事件 
```
constructor(props) {
  super(props);
  this.handleChange = this.handleChange.bind(this);
} // 第二种
handleChange = () => {} // 第一种
<input onChange={this.handleChange} /> // this指向
<input onChange={() => this.handleChange(user)} /> // 第三种
```

#### 通信
```
// 属性方式通信（父组件复杂，子组件负责调用）
// 父子组件隔代，上下文关系，及redux
<Comp titile={} onSubmit={this.onSubmit} />
```

#### React VScode开发快捷键
* 按照扩展插件 ** ES7 React/Redux/GraphQL/React-Native snippets **
* 几个常用命令
* rcc 快速穿件一个组件（使用extends方式）
* rconst 快速创建一个 constuctor
* rcep 快速创建一个组件（使用extends方式）
* rcredux 快速创建一个 redux格式的类模板
* clg 是 console.log()的快捷键
[To Learn More...](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)
