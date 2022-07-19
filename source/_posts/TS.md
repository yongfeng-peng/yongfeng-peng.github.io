---
title: TS
date: 2022-03-30 14:54:03
categories:
- JS
tags:
---

### typeScript
* <a href="https://www.typescriptlang.org/zh/" style="color: blue;">Typescript</a> 
* <a href="https://github.com/nvm-sh/nvm" style="color: blue;">使用 nvm 来管理 node 版本</a> 
#### 为什么要学习ts
* 程序更容易理解
  * 函数或者方法输入输出的参数类型、外部条件
  * 动态语言的约束：需要手动调试等过程
  * ts可以解决以上问题
* 效率更高
  * 在不同的代码块和定义中进行跳转
  * 代码自动补全
  * 丰富的接口提示
* 更少的错误
  *  编译期间能够发现大部分错误
  * 杜绝一些比较常见的错误
* 非常好的包容性
  * 完全兼容javascript
  * 第三方库可以单独编写类型文件
  * 大多数项目都支持ts
* 缺点
  * 增加一些学习成本
  * 短期内增加了一些开发成本
* 安装 Typescript:
```
npm install -g typescript
yarn global add typescript
```
* 使用 tsc 全局命令：
```
// 查看 tsc 版本
tsc -v
// 编译 ts 文件
tsc fileName.ts
// 运行
ts-node fileName.ts
```
* 数组和元组
```
//最简单的方法是使用「类型 + 方括号」来表示数组：
let arrOfNumbers: number[] = [1, 2, 3, 4]
//数组的项中不允许出现其他的类型：
//数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：
arrOfNumbers.push(3)
arrOfNumbers.push('abc')

// 元祖的表示和数组非常类似，只不过它将类型写在了里面 这就对每一项起到了限定的作用
let user: [string, number] = ['viking', 20]
//但是当我们写少一项 就会报错 同样写多一项也会有问题
user = ['molly', 20, true]
```
* interface 接口
```
// 我们定义了一个接口 Person
interface Person {
  name: string;
  age: number;
}
// 接着定义了一个变量 viking，它的类型是 Person。这样，我们就约束了 viking 的形状必须和接口 Person 一致。
let viking: Person ={
  name: 'viking',
  age: 20
}

//有时我们希望不要完全匹配一个形状，那么可以用可选属性：
interface Person {
    name: string;
    age?: number;
}
let viking: Person = {
    name: 'Viking'
}

//接下来还有只读属性，有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 readonly 定义只读属性

interface Person {
  readonly id: number;
  name: string;
  age?: number;
}
viking.id = 9527
```
* 函数
```
// 来到我们的第一个例子，约定输入，约定输出
function add(x: number, y: number): number {
  return x + y
}
// 可选参数
function add(x: number, y: number, z?: number): number {
  if (typeof z === 'number') {
    return x + y + z
  } else {
    return x + y
  }
}

// 函数本身的类型
const add2: (x: number, y: number, z?:number) => number = add

// interface 描述函数类型
const sum = (x: number, y: number) => {
  return x + y
}
interface ISum {
  (x: number, y: number): number
}
const sum2: ISum = sum
```
* 类型推论，联合类型 和 类型断言
```
// 联合类型 - union types
// 我们只需要用中竖线来分割两个
let numberOrString: number | string 
// 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法：
numberOrString.length
numberOrString.toString()
// 类型断言 - type assertions
// 这里我们可以用 as 关键字，告诉typescript 编译器，你没法判断我的代码，但是我本人很清楚，这里我就把它看作是一个 string，你可以给他用 string 的方法。
function getLength(input: string | number): number {
  const str = input as string
  if (str.length) {
    return str.length
  } else {
    const number = input as number
    return number.toString().length
  }
}
// 类型守卫 - type guard
// typescript 在不同的条件分支里面，智能的缩小了范围，这样我们代码出错的几率就大大的降低了。
function getLength2(input: string | number): number {
  if (typeof input === 'string') {
    return input.length
  } else {
    return input.toString().length
  }
}
```
* 枚举 Enums
```
// 数字枚举，一个数字枚举可以用 enum 这个关键词来定义，我们定义一系列的方向，然后这里面的值，枚举成员会被赋值为从 0 开始递增的数字,
enum Direction {
  Up,
  Down,
  Left,
  Right,
}
console.log(Direction.Up)

// 还有一个神奇的点是这个枚举还做了反向映射
console.log(Direction[0])

// 字符串枚举
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT',
}
const value = 'UP'
if (value === Direction.Up) {
  console.log('go up!')
}
```
* 泛型 Generics
  * 泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性
  * 相当于一个占位符
```
function echo(arg) {
  return arg
}
const result = echo(123)
// 这时候我们发现了一个问题，我们传入了数字，但是返回了 any

function echo<T>(arg: T): T {
  return arg
}
const result = echo(123)

// 泛型也可以传入多个值
function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]]
}

const result = swap(['string', 123])
```
* 泛型约束
  * 在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法
```
function echoWithArr<T>(arg: T): T {
  console.log(arg.length)
  return arg
}

// 缺陷 只能指定特定类型
// function echoWithArr<T>(arg: T[]): T[] {
//   console.log(arg.length)
//   return arg
// }
// 上例中，泛型 T 不一定包含属性 length，我们可以给他传入任意类型，当然有些不包括 length 属性，那样就会报错

interface IWithLength {
  length: number;
}
function echoWithLength<T extends IWithLength>(arg: T): T {
  console.log(arg.length)
  return arg
}

echoWithLength('str')
const result3 = echoWithLength({length: 10})
const result4 = echoWithLength([1, 2, 3])
```
* 泛型与类和接口
```
class Queue {
  private data = [];
  push(item) {
    return this.data.push(item)
  }
  pop() {
    return this.data.shift()
  }
}

const queue = new Queue()
queue.push(1)
queue.push('str')
console.log(queue.pop().toFixed())
console.log(queue.pop().toFixed())

//在上述代码中存在一个问题，它允许你向队列中添加任何类型的数据，当然，当数据被弹出队列时，也可以是任意类型。在上面的示例中，看起来人们可以向队列中添加string 类型的数据，但是那么在使用的过程中，就会出现我们无法捕捉到的错误，

class Queue<T> {
  private data = [];
  push(item: T) {
    return this.data.push(item)
  }
  pop(): T {
    return this.data.shift()
  }
}
const queue = new Queue<number>()

//泛型和 interface
interface KeyPair<T, U> {
  key: T;
  value: U;
}

let kp1: KeyPair<number, string> = { key: 1, value: "str"}
let kp2: KeyPair<string, number> = { key: "str", value: 123}
```
* 类型别名(Type Aliases) 和 交叉类型( Intersection Types)
  * 类型别名，就是给类型起一个别名，让它可以更方便的被重用

```
let sum: (x: number, y: number) => number
const result = sum(1,2)
type PlusType = (x: number, y: number) => number
let sum2: PlusType

// 支持联合类型
type StrOrNumber = string | number
let result2: StrOrNumber = '123'
result2 = 123

// 字符串字面量
type Directions = 'Up' | 'Down' | 'Left' | 'Right'
let toWhere: Directions = 'Up'
```

```
interface IName  {
  name: string
}
type IPerson = IName & { age: number }
let person: IPerson = { name: 'hello', age: 12}
```
* <a href="https://www.imooc.com/wiki/vue3TS/w20c7.html" style="color: blue;">more...</a> 