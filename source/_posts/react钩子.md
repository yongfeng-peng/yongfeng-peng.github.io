---
title: react钩子
date: 2020-12-28 15:23:21
categories:
- 前端框架
tags:
---

### react钩子（hooks）
#### react组件
* React 的核心是组件。v16.8 版本之前，组件的标准写法是类（class）
* Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性
* 任何一个组件都可以用类和钩子来写
* 类
```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
* 钩子
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
* 大部分看到上面两段代码都会选择使用钩子，更简洁、代码量少、用起来也较轻，也更符合react*函数式都本质*
  * 类有很多强制性的语法约束、不易混乱，更适合初学者
  * 真实的 React App 由多个类按照层级，一层层构成，复杂度成倍增长。再加入 Redux，就变得更复杂
* 而类就比较重，官方也推荐使用钩子（函数），而不是类
  * 钩子的灵活性比较大
  * react 团队希望，组件不要变成复杂的容器，最好只是数据流的管道。开发者根据需要，组合管道即可。 组件的最佳写法应该是函数，而不是类

#### 类、函数的差异
* 毕竟写法不同，还是存在不同的编程方法论
* 类是**数据和逻辑的封装**，即组件的状态和操作方法是封装在一起的
* 函数一般来说，只应该做一件事，就是返回一个值，如：react 的函数组件只应该做一件事情=》返回组件的 HTML 代码，而没有其他的功能
* 这种只进行单纯的数据计算（换算）的函数，在函数式编程里面称为 "纯函数"（pure function）

#### 副效应
* 函数式编程将那些跟数据计算无关的操作，都称为 "副效应" （side effect） ，
* 如果函数内部直接包含产生副效应的操作，就不再是纯函数了，我们称之为不纯的函数，如（生成日志、储存数据、改变应用状态等等）
* 纯函数内部只有通过间接的手段（即通过其他函数调用），才能包含副效应，如（钩子）

#### 钩子（hook）
* **react函数组件的副效应解决方案，用来为函数组件引入副效应**. 函数组件的主体只应该用来返回组件的 HTML 代码，所有的其他操作（副效应）都必须通过钩子引入
* React 为许多常见的操作（副效应），都提供了专用的钩子。
  * useState()：保存状态
  * useContext()：保存上下文（共享状态钩子）
    * 组件之间共享状态，可以使用useContext()
    * 使用 React Context API，在组件外部建立一个 Context
    ```
    const AppContext = React.createContext({});
    ```
    * AppContext.Provider提供了一个 Context 对象，这个对象可以被子组件共享
    * useContext()钩子函数用来引入 Context 对象，从中获取username属性
  * useRef()：保存引用
  * useReducer()：action 钩子
    * React 本身不提供状态管理功能，通常需要使用外部库。这方面最常用的库是 Redux
    * Redux 的核心概念是，组件发出 action 与状态管理器通信。状态管理器收到 action 以后，使用 Reducer 函数算出新的状态，Reducer 函数的形式是(state, action) => newState
    * useReducer()的基本用法，它接受 Reducer 函数和状态的初始值作为参数，返回一个数组。数组的第一个值是状态的当前值，第二个值是发送 action 的dispatch函数
    ```
    import React, { useContext } from 'react';
    import ReactDOM from 'react-dom';
    const [state, dispatch] = useReducer(reducer, initialState);
    const myReducer = (state, action) => {
      switch(action.type)  {
        case('countUp'):
          return  {
            ...state,
            count: state.count + 1
          }
        default:
          return  state;
      }
    }
    function App() {
      const [state, dispatch] = useReducer(myReducer, { count:   0 });
      return  (
        <div className="App">
          <button onClick={() => dispatch({ type: 'countUp' })}>
            +1
          </button>
          <p>Count: {state.count}</p>
        </div>
      );
    }
    ```
    * Hooks 可以提供共享状态和 Reducer 函数，所以它在这些方面可以取代 Redux。但是，它没法提供中间件（middleware）和时间旅行（time travel），如果你需要这两个功能，还是要用 Redux
  * 以上钩子都是引入某种特定的副效应，而 useEffect()是通用的副效应钩
  * [......](https://react.docschina.org/docs/hooks-reference.html)

#### useEffect() 的用法
* useEffect Hook 看做 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合
* useEffect()本身是一个函数，由 React 框架提供，在函数组件内部调用即可
* useEffect()的作用就是指定一个副效应函数，组件每渲染一次，该函数就自动执行一次。组件首次在网页 DOM 加载后，副效应函数也会执行
```
import React, { useEffect } from 'react';

function Welcome(props) {
  let [count, setCount] = useState(1); // 数组解构
  // 以上代码等价于
  // let countStateVariable = React.useState(1); // 返回一个有两个元素的数组
  // let count = countStateVariable[0]; // 数组里的第一个值 当前的state
  // let setCount = countStateVariable[1]; // 数组里的第二个值 更新state的函数
  // 使用 [0] 和 [1] 来访问有点令人困惑，因为它们有特定的含义。这就是我们使用数组解构的原因。

  useEffect(() => {
    document.title = `you click ${count} times`;
  },)
  return <div>
    <h1>hello, { props.title },{count}</h1>
    <button onClick={() => setCount(count + 1)}>累加</button>
  </div>;
}
```

* useEffect() 的第二个参数,不希望useEffect()每次渲染都执行，这时可以使用它的第二个参数，使用一个数组指定副效应函数的依赖项，只有依赖项发生变化，才会重新渲染
```
function Welcome(props) {
  useEffect(() => {
    document.title = `Hello, ${props.name}`;
  }, [props.name]);
  return <h1>Hello, {props.name}</h1>;
}
```

### useEffect() 的用途
* 只要是副效应，都可以使用useEffect()引入。它的常见用途有下面几种。
  * 获取数据（data fetching）
  * 事件监听或订阅（setting up a subscription）
  * 改变 DOM（changing the DOM）
  * 输出日志（logging）

#### useEffect() 的返回值
* 副效应是随着组件加载而发生的，那么组件卸载时，可能需要清理这些副效应
* useEffect()允许返回一个函数，在组件卸载时，执行该函数，清理副效应。如果不需要清理副效应，useEffect()就不用返回任何值
* useEffect()在组件加载时订阅了一个事件，并且返回一个清理函数，在组件卸载时取消订阅。
```
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    subscription.unsubscribe();
  };
}, [props.source]);
```

#### useEffect() 的注意点
* [了解Hook规则](https://react.docschina.org/docs/hooks-rules.html)
* 使用useEffect()时，有一点需要注意。如果有多个副效应，应该调用多个useEffect()，而不应该合并写在一起
```
function App() {
  const [varA, setVarA] = useState(0);
  const [varB, setVarB] = useState(0);

  useEffect(() => {
    const timeout = setTimeout(() => setVarA(varA + 1), 1000);
    return () => clearTimeout(timeout);
  }, [varA]);

  useEffect(() => {
    const timeout = setTimeout(() => setVarB(varB + 2), 2000);

    return () => clearTimeout(timeout);
  }, [varB]);

  return <span>{varA}, {varB}</span>;
}
```
#### [learn more...](http://www.ruanyifeng.com/blog/2020/09/react-hooks-useeffect-tutorial.html)
#### [learn more...](http://www.ruanyifeng.com/blog/2019/09/react-hooks.html)
### [code](https://github.com/yongfeng-peng/react-simple-sCode)
