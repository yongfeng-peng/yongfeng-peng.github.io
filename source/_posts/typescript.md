---
title: typescript
date: 2021-01-14 10:14:12
categories:
- JS
tags:
---

### typeScript
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