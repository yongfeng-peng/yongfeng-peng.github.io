---
title: 类与继承
date: 2019-04-30 10:17:53
tags:
---

#### 如何实现继承及继承的几种方式
##### 借助构造函数实现继承
```
function Parent() {
    this.name = 'parent1';
}
Parent.prototype.say = function(){};
function Child() {
    Parent.call(this);
    this.type = 'child';
}
console.log(new Child); // 无参数，（）可以省略
```
* 原理：子类通过**call方法**改变函数运行上下文
* 不足：把Parent1的this指向Child1,**不能继承父类的原型上的方法**

##### 借助构造函数实现继承
```
function Parent2() {
    this.name = 'parent2';
    this.play = [1, 2, 3];
}
function Child2() {
    this.type = 'child2';
}
Child2.prototype = new Parent2(); 
console.log(new Child2().__proto__);
console.log(new Child2().__proto__ === Child2.prototype); // true
console.log(new Child2().__proto__.name); // parent2
var s1 = new Child2();
var s2 = new Child2();
console.log(s1.play, s2.play);
s1.play.push(4);
console.log(s1.play, s2.play);
console.log(s1.__proto__ === s2.__proto__); // true
```
* 原理：子类通过**prototype属性**指向父类的实例
* 不足：修改s1,s2同时也改变，事实应该是隔离的，**原型链中的原型对象是公用的**

##### 组合方式继承
```
function Parent3() {
    this.name = 'parent3';
    this.play = [1, 2, 3];
}
function Child3() {
    Parent3.call(this);
    this.type = 'child3';
}
Child3.prototype = new Parent3();
var s3 = new Child3();
var s4 = new Child3();
s3.play.push(4);
console.log(s3.play, s4.play);
console.log(s3.constructor);
```
* 不足：父类构造函数执行了两次

##### 组合方式继承优化1
```
function Parent4() {
    this.name = 'parent4';
    this.play = [1, 2, 3];
}
function Child4() {
    Parent4.call(this);
    this.type = 'child4';
}
Child4.prototype = Parent4.prototype;
var s5 = new Child4();
var s6 = new Child4();
console.log(s5, s6);
console.log(s5 instanceof Child4, s5 instanceof Parent4);
console.log(s5.constructor);
```

##### 组合方式继承优化2
```
function Parent5() {
    this.name = 'parent5';
    this.play = [1, 2, 3];
}
function Child5() {
    Parent5.call(this);
    this.type = 'child5';
}
Child5.prototype = Object.create(Parent5.prototype); // 创建中间对象__proto__)
Child5.prototype.constructor = Child5;
var s7 = new Child5();
console.log(s7 instanceof Child5, s7 instanceof Parent5);
console.log(s7.constructor);
```