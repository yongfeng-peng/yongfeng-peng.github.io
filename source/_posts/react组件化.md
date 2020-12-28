---
title: react组件化
date: 2020-12-21 10:46:03
categories:
- 前端框架
tags:
---

### react组件化
#### 使用antd的yarn create react-app antd-demo创建的项目，使用组件配置按需加载
* 安装react-app-rewired取代react-scripts(package.json)，可以扩展webpack的配置 ，类似vue.config.js
* 项目根目录创建 config-overrides.js，配置
```
const { injectBabelPlugin } = require("react-app-rewired");
module.exports = function override(config, env) {
  config = injectBabelPlugin(
    // 在默认配置基础上注入
    // 插件名，插件配置
    ["import", { libraryName: "antd", libraryDirectory: "es", style: "css" }],
    config
  );
  // 高级组件、可使用装饰器配置 ES7 -> ES5
  config = injectBabelPlugin(
    ["@babel/plugin-proposal-decorators", { legacy: true }],
    config
  );

  return config;
};

```
* 使用Button实例
```
import React, { Component } from 'react'
// antd.css都加载
// import Button from 'antd/lib/button'
// import 'antd/dist/antd.css'
// 按需加载
import {Button} from 'antd'

export default class AntdTest extends Component {
  render() {
    return (
      <div>
        <Button type="primary">按钮</Button>
      </div>
    )
  }
}
```

#### 容器组件、函数组件，react渲染数据的深浅比较
* 容器组件负责数据获取，展示组件负责根据props显示信息
* shouldComponentUpdate
* PureComponent (定制了shouldComponentUpdate后的Component（浅比较）)
```
class Comp extends React.PureComponent {}
```
* memo高阶组件(React v16.6.0 之后的版本，可以使用 React.memo 让函数式的组件也有PureComponent的功能)
```
const Demo = React.memo(() => ( <div>{this.props.value || 'loading...' } </div> ));
```
* 实例
```
import React, { Component } from "react";
import { func } from "prop-types";

// 容器组件
export default class CommentList extends Component {
  constructor(props) {
    super(props);
    this.state = {
      comments: []
    };
  }
  componentDidMount() {
    // 不断轮询比较、耗费🆓
    // setInterval(() => { // 每次生成的是全新数组
    setTimeout(() => {
      this.setState({
        comments: [
          { body: "react is very good", author: "facebook" },
          { body: "vue is very good", author: "youyuxi" }
        ]
      });
    }, 1000);
  }
  render() {
    return (
      <div>
        {this.state.comments.map((c, i) => (
          <Comment key={i} data={c} />
          // <Comment key={i} body={c.body} author={c.author} />
          // <Comment key={i} {...c} />
        ))}
      </div>
    );
  }
}
// 展示组件
// memo高阶组件（也是一个函数，接收一个组件、返回一个全新的组件）,也是浅比较（实现原理和使用PureComponent一样，解决了以前函数组件不能使用PureComponent）
// React v16.6.0 之后的版本，可以使用 React.memo 让函数式的组件也有PureComponent的功能
// const Comment = React.memo(function(props) {
//   console.log("render Comment"); // 执行两次
//   return (
//     <div>
//       <p>{props.body}</p>
//       <p> --- {props.author}</p>
//     </div>
//   );
// });

// 使用PureComponent （15.3 版本） 浅比较，避免对象比较（引用类型），挖掘太深，也没有达到效果
// 解决 数据没有变 render函数不执行
// 传值类型、或者避免引用类型的地址不变（尽量一层）
// class Comment extends React.PureComponent{
//   render() {
//     console.log("render comment");
//     return (
//       <div>
//         <p>{this.props.body}</p>
//         <p> --- {this.props.author}</p>
//       </div>
//     );
//   }
// }

// class Comment extends React.Component{
//   // 生命周期进行比较 新状态nextProps 与 data比较
//   // 弊端： 比较累赘
//   shouldComponentUpdate(nextProps){
//       if (nextProps.data.body === this.props.data.body &&
//         nextProps.data.author === this.props.data.author) {
//           return false;
//       }
//       return true;
//   }

//   render() {
//     console.log("render comment"); // 执行了2次
//     return (
//       <div>
//         <p>{this.props.data.body}</p>
//         <p> --- {this.props.data.author}</p>
//       </div>
//     );
//   }
// }

function Comment({data}){
  console.log("render comment"); // 执行了4次
  return (
    <div>
      <p>{data.body}</p>
      <p> --- {data.author}</p>
    </div>
  );
}

```

#### 高阶组件
* 在React里就有了HOC（Higher-Order Components）的概念
* 高阶组件也是一个组件，但是他返回另外一个组件，产生新的组件可以对属性进行包装，甚至重写部分生命周期
```
const demoComp = (Component) => { 
  const NewComponent = (props) => { 
    return <Component {...props} name='demo组件' />; 
  };
    return NewComponent; 
  };
// 上面demoComp组件，其实就是代理了Component，只是多传递了一个name参数
```

#### 高阶链式调用
* 高阶组件最巧妙的一点，是可以链式调用

#### 高阶组件装饰器写法
* ES7装饰器可用于简化高阶组件写法
* npm install --save-dev babel-plugin-transform-decorators-legacy
* 同上，项目根目录创建 config-overrides.js，配置
* 参考源码 https://github.com/yongfeng-peng/my-react-demo/blob/master/src/components/Hoc.js
```
// 高级组件、可使用装饰器配置 ES7 -> ES5
config = injectBabelPlugin(
  ["@babel/plugin-proposal-decorators", { legacy: true }],
  config
);
```

#### 组件跨层级通信 - 上下文
* 组件跨层级通信可使用Context
* 这种模式下有两个角色，Provider和Consumer
* Provider为外层组件，用来提供 数据；内部需要数据时用Consumer来读取
```
const FormContext = React.createContext()
const FormProvider = FormContext.Provider
const FormConsumer = FormContext.Consumer
let store = { 
  name: '学习', 
  sayHi() { 
    console.log(this.name);
  } 
}
let withForm = Component=> { 
  const NewComponent = (props) => { 
    return <FormProvider value={store}> <Component {...props} /> </FormProvider> 
  };
  return NewComponent; 
}
@withForm class App extends Component { 
  render() { 
    return <FormConsumer> { 
      store=> { 
        return <Button onClick={()=>store.sayHi()}> {store.name} </Button> 
      } 
    } </FormConsumer> 
  } 
}

```
#### [code](https://github.com/yongfeng-peng/my-react-demo)
