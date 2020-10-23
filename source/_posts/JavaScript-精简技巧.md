---
title: JavaScript 精简技巧
date: 2020-09-14 14:47:30
categories:
- JS
tags: JavaScript 精简技巧
---
---

#### JavaScript 精简技巧
##### 快速浮点数转整数
* 将浮点数转换为整数，可以使用Math.floor()、Math.ceil()或Math.round()。但是还有一种更快的方法可以使用|(位或运算符)将浮点数截断为整数。
```
console.log(23.2 | 0); // Result: 23

```
