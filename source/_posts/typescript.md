---
title: typescript
date: 2021-01-14 10:14:12
categories:
- JS
tags:
---

### typeScript
#### 发展
* 任何能有js实现的应用最终都会使用js实现
* 从移动终端到后端服务
* 从IOT到神经网络，它几乎无处不在，如此广阔的应用领域，自然对语言的安全性、健壮行、可维护性有更高的要求，尽管ES标准在近几年有长足的进步，但在类型检查方面依然无所建树
* 你是否经常遇到这样但场景
  * 调用一个别人写但函数，很不幸，没有留下任何注释，为了搞清楚参数类型，硬着头皮看逻辑
  * 为了保证代码的健壮性，对一个函数的输入参数进行各种假设
  * 领导看好你，维护一个重要的底层类库，你殚精竭虑优化了一个参数类型，但不知道有多少处引用，是否在提交代码前，感到脊背发凉
  * 明明定义好接口，可一联调就报错了，不能阅读到length
* 以上归根结底，js是一门动态弱类型语言，对变量的类型非常宽容，不会在这些变量和他们的调用者之间建立结构化的契约，如果长期在没有类型约束的环境下开发，造成类型思维的缺失

#### 定义
* 官方定义：拥有类型系统的JavaScript的超集，可以编译成纯的JavaScript
* 类型检查：编译代码时，进行严格的静态类型检查，即编码阶段发现一些可避免的隐患，不必带到生产
* 语言扩展：包括ES6、未来提案中的特性，如异步操作、装饰器，也会从其它语言借鉴某些特性，如接口、抽象类
* 工具属性：ts可以编译成标准的js，可以在任何浏览器、操作系统上运行，无需任何运行时的额外开销，从这个角度讲，ts更像一个工具而非语言
#### 使用ts好处
* vs code具有强大的自动补全、导航、重构功能，使得接口定义可以直接代替文档，同时可以提高开发效率，降低维护成本，更重要的是ts可以帮助团队，重塑**类型思维**，接口的提供方将被迫去思考API的边界，将从代码的编写者蜕变为代码的设计者
* 如果js是野马，ts就是束缚这匹野马的缰绳，作为骑士的你，自然可以张开双臂、放飞自我，但如果不是技艺超群，恐怕会摔得很惨，然而如果抓住缰绳，你即可闲庭信步，亦可策马扬鞭，这就是ts的价值，它可让你在前端开发路上走得更稳，更远

#### 安装 TypeScript 并运行 ts 文件
```
* npm install typescript -g
* yarn global add typescript
* 使用 tsc 命令转成 js 文件
  * tsc xx.ts
  * node xx.js
* 直接运行ts文件可以使用ts-node
  * 安装ts-node
  * npm install ts-node -g
  * 使用
  * ts-node xx.ts
```

#### TS基础数据类型
```
// string
let s: string = 'aaa';
console.log(s)

// number
let num: number = 123;
console.log(num)

// 不声明具体数据类型，由代码推断出数据类型
let a = '122';
console.log(a)

// 布尔
let b: boolean = false;
console.log(b)

// 数组 数据类型[]
let arr: string[] = ['11', '22'];
console.log(arr)

// void
function funA(): void {
  console.log('定义了一个没有返回值的函数')
}
funA();

// function funNum(): number {
//   console.log('定义了一个有返回值的函数')
//   return 1;
// }

// null
// undefined

```

#### TS接口
```
// 接口的声明
interface inf {

}

// 接口必须的成员变量
interface inf {
  name: string
}

// 接口可选的成员变量，可以有，可以没有
interface inf {
  age?: number
}

// 复杂的接口定义
interface inf {
  name: string
  age?: number
  say: () => void
  userList: string[]
}

// 接口的继承
interface child extends inf {

}
```

#### TS泛型
```
class fx {
  name: string
  age: number
  say() {
    console.log(`${this.name}---${this.age}岁`)
  }
}

let f = new fx();
f.name = 'heihei';
f.age = 18;
f.say();

class fx1<IProps = {}> {
  user: IProps
  say() {
    console.log(this.user);
  }
}

interface IUser {
  name: string
  age: number
}

// 泛型只关注为需要的数据，不关注具体的数据格式
let f1 = new fx1<IUser>();
f1.user = {age: 18, name: 'aa'};
f1.say();
```

#### TS枚举
```
enum Direction {
  UP = 'up',
  DOWN = 'down',
  LEFT = 'left',
  RIght = 'right'
}

enum User {
  UP = 'up',
}

console.log(Direction.UP)
// 简单理解 有自己的独立作用域
// console.log(Direction.UP === User.UP)

const a = 1;
const b = 1;
console.log(a == b)

```