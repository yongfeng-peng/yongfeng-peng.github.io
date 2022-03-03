---
title: for循环和forEach的语法
date: 2022-03-05 15:20:22
categories:
- js
tags:
---

### for循环和forEach的语法
* <a href="https://mp.weixin.qq.com/s/9UR22JFovfJegmhhUPMjdg" style="color: blue;">To Learn More</a> 

#### for循环和forEach的本质区别
* forEach 是负责遍历（Array Set Map）可迭代对象的，而 for 循环是一种循环机制，只是能通过它遍历出数组
* 只要是可迭代对象，调用内部的 Symbol.iterator 都会提供一个迭代器，并根据迭代器返回的next 方法来访问内部，这也是 for...of 的实现原理

```
  let arr = [1, 2, 3, 4]  // 可迭代对象
  let iterator = arr[Symbol.iterator]()  // 调用 Symbol.iterator 后生成了迭代器对象
  console.log(iterator.next()); // {value: 1, done: false}  访问迭代器对象的next方法
```

#### for循环和forEach的语法区别
* forEach 的参数
* arr.forEach((self,index,arr) =>{},this) // this： 回调函数中this指向
```
  let arr = [1, 2, 3, 4];
  let person = {
      name: 'xx'
  };
  arr.forEach(function (self, index, arr) {
      console.log(`当前元素为${self}索引为${index},属于数组${arr}`);
      console.log(this.name+='美美哒');
  }, person)

```
* arr 实现数组去重
```
  let arr1 = [1, 2, 1, 3, 1];
  let arr2 = [];
  arr1.forEach(function (self, index, arr) {
      arr.indexOf(self) === index ? arr2.push(self) : null;
  });
  console.log(arr2);   // [1,2,3]

```

* forEach 的中断
* js中有break return continue 对函数进行中断或跳出循环的操作(优化数组遍历查找),但forEach属于迭代器，只能按序依次遍历完成，不支持上述的中断行为
```
  let arr = [1, 2, 3, 4],
      i = 0,
      length = arr.length;
  for (; i < length; i++) {
      console.log(arr[i]); //1,2
      if (arr[i] === 2) {
          break;
      };
  };

  arr.forEach((self,index) => {
      console.log(self);
      if (self === 2) {
          break; //报错
      };
  });

  arr.forEach((self,index) => {
      console.log(self);
      if (self === 2) {
          continue; //报错
      };
  });
```
*  forEach 中跳出循环呢？其实是有办法的，借助try/catch
```
  try {
      var arr = [1, 2, 3, 4];
      arr.forEach(function (item, index) {
          //跳出条件
          if (item === 3) {
              throw new Error("LoopTerminates");
          }
          //do something
          console.log(item);
      });
  } catch (e) {
      if (e.message !== "LoopTerminates") throw e;
  };
```
* return 并不会报错，但是不会生效
```
  let arr = [1, 2, 3, 4];
  function find(array, num) {
      array.forEach((self, index) => {
          if (self === num) {
              return index;
          };
      });
  };
  let index = find(arr, 2);// undefined
```

* forEach 删除自身元素，index不可被重置
```
  let arr = [1,2,3,4]
  arr.forEach((item, index) => {
      console.log(item); // 1 2 3 4
      index++;
  });
```

* for 循环可以控制循环起点
```
  let arr = [1, 2, 3, 4],
      i = 1,
      length = arr.length;

  for (; i < length; i++) {
      console.log(arr[i]) // 2 3 4
  };
```
* 数组遍历并删除
```
  let arr = [1, 2, 1],
      i = 0,
      length = arr.length;

  for (; i < length; i++) {
      // 删除数组中所有的1
      if (arr[i] === 1) {
          arr.splice(i, 1);
          //重置i，否则i会跳一位
          i--;
      };
  };
  console.log(arr); // [2]
  //等价于
  var arr1 = arr.filter(index => index !== 1);
  console.log(arr1) // [2]
```

* for循环和forEach的性能区别
  * 性能比较：for > forEach > map 在chrome 62 和 Node.js v9.1.0环境下：for 循环比 forEach 快1倍，forEach 比 map快20%左右
  * for循环没有额外的函数调用栈和上下文，所以它的实现最为简单

