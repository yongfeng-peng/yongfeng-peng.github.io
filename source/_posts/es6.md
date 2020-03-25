---
title: ES6
date: 2019-05-05 10:03:29
tags:
---

#### ES6 语法及应用
<a href="https://jscode.me/t/topic/109" style="color: blue;">es6学习可参照</a>
##### 作用域
* 块级作用域： 由一对大括号{}界定
* 块级变量 let：声明一个块级作用域的本地变量，可选的将其初始化为一个值
* 块级常量 const: 常量的值不能通过重新赋值来改变，并且不能重新声明(** 是否指向同一个内存地址 **)

##### let、const与var的区别
* var
    * 可以重复定义
    * 可以随意更改
    * 没有块级作用域
* let与const
    * 在同一个块级作用域中，不允许重复定义
    * const定义的变量不允许二次修改
    * let和const定义的变量会形成块级作用域
    * 它们定义的变量不存在变量提升，以及存在暂时性死区

##### 箭头函数
* sum = (a, b) => a + b
* nums.forEach( v => { console.log(v) })
* 强大之处 this指向不会更改

##### 参数处理
* 默认参数值: 允许在没有值或undefined被传入时使用默认形参
```
function multiply(a, b = 1) {
  return a * b;
}
console.log(multiply(5, 2)); // 10
console.log(multiply(5)); // 5
```
* 剩余参数: 允许将一个不定数量的参数（theArgs）表示为一个数组
```
function sum(...theArgs) {
  return theArgs.reduce((previous, current) => {
    return previous + current;
  });
}
console.log(sum(1, 2, 3)); // 6
console.log(sum(1, 2, 3, 4)); // 10
```
* 展开运算符: 函数调用时，将表达式(数组/对象-简单的数据类型)做对应语法的展开
```
function sum(x, y, z) {
  return x + y + z;
}
const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
console.log(sum.apply(null, numbers)); // 6
```
##### 模板字面量：允许嵌入表达式的字符串字面量, 反引号包裹字符串
* 多行字符串 `字符串内容`
* 字符串插值 `${字符串变量}`
* 带标签的模板字面量
* 原始字符串

##### 解构赋值: 将值从数组，或属性从对象，提取到不同的变量中
* 数组匹配 [ b, a ] = [ a, b ]
* 对象匹配 let obj = {a: 1, b: 23}; let { a, b, c } = obj;  
* 参数匹配 function fun({ name: n, val: v }) {}

##### 模块
* <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import" style="color: blue;">导入import</a> ：导入由另一个模块导出的绑定
    * import {foo, bar as b} from '/xxx.js';
    * 常使用导入多个接口，或者重命名接口名称都很实用
* <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/export" style="color: blue;">导出export</a>
* 默认导出 export default: export default function(){} / export default class {}

##### 类
* 使用 extends 实现继承
* 重写构造器
* super 关键字

###### 是否有class关键字实现继承
```
function Animal(color){
    this.color = color
}
Animal.prototype.move = function() {}
function Dog(color, name) {
    Animal.call(this, color); // 或者 Animal.apply(this, arguments)
    this.name = name;
}
// 下面三行实现 Dog.prototype.__proto__ = Animal.prototype
function temp() {}
temp.prototye = Animal.prototype;
Dog.prototype = new temp();
Dog.prototype.constuctor = Dog;
Dog.prototype.say = function(){ console.log('汪')}
var dog = new Dog('白色','咪汪');
```
```
class Animal {
    constructor(color) {
        this.color = color
    }
    move(){}
}
class Dog extends Animal {
    constructor(color, name) {
        super(color)
        this.name = name
    }
    say() {}
}
```

##### 原有内置对象 API 增强
* 常用
    * Object.assign：将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。Object.assign(target, ...source);
    * Array.from: 从一个类似数组或可迭代对象中创建一个新的数组实例。Array.from(arrayLike[, mapFn[, thisArg]])

#####  Promise
###### Promise: 一个异步操作的最终状态（完成或失败），以及该异步操作的结果值
    * pending: 初始状态，既不是成功，也不是失败状态。
    * fulfilled: 操作成功完成。
    * rejected: 操作失败。
```
function fn(){
    return new Promise((resolve, reject)=>{
        成功时调用 resolve(数据)
        失败时调用 reject(错误)
    })
}
fn().then(success, fail).then(success2, fail2)
```
###### Promise.all 
* promise1和promise2**都成功**，才会调用success1
* promise1和promise2有**一个失败**，都会调用fail1
```
Promise.all([promise1, promise2]).then(success1, fail1)
```
###### Promise.race 
* promise1和promise2只要有**一个成功**就会调用success1
* promise1和promise2**都失败**，才会调用fail1
```
Promise.race([promise1, promise2]).then(success1, fail1)
```

#####  手写一个 Promise <a href="https://juejin.im/post/5aafe3edf265da238f125c0a" style="color: blue;">参考</a> 

##### async 与 await
* 与Promise相关，
* 如果是异步函数， 它的语法和结构会更像是标准的**同步函数**
1. promise实例
```
function fn() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      let n = parseInt(Math.random() * 6 + 1, 10); // [0, 5)
      resolve(n);
    }, 3000);
  });
}
fn().then(
  (n) => {
    console.log('成功返回骰子数' + n);
  },
  (err) => {
    console.log('失败返回!');
  }
);
```
2. await的简单使用
```
async function test() {
  // test的执行，或者n的赋值一定是在3s之后才执行
  let n = await fn();
  console.log(n);
}
test();
```
3. await语法中error错误处理
```
function fn(params) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      let n = parseInt(Math.random() * 6 + 1, 10); // [0, 6)
      if (n > 3) {
        if (params === ‘大’) {
          resolve(n);
        } else {
          reject(n);
        }
      } else {
        if (params === ‘小’) {
          resolve(n);
        } else {
          reject(n);
        }
      }
    }, 3000);
  });
}
async function test() {
  try {
    let n = await fn('大');
    console.log('猜中了' + n);
  } catch (error) {
    console.log('输了' + error);
  }
}
test();
```
4. 同时调用fn函数，Promise处理
```
Promise.all([fn('大'), fn('大')]).then(
  res => {
    console.log(res);
  },
  error => {
    console.log(error);
  }
);
```
5. 同时调用fn函数，await处理
```
async function test() {
  // test的执行，或者n的赋值一定是在3s之后才执行
  try {
    let n = await Promise.all([fn('大'), fn('大')]);
    console.log('猜中了' + n);
  } catch (error) {
    console.log('输了' + error);
  }
}
test();
```
    async与await返回的都是Promise,async与await可理解为Promise的语法糖
    <a href="http://es6.ruanyifeng.com/?search=async&x=0&y=0#docs/async" style="color: blue;">想了解更多，看阮一峰-es6教程</a>