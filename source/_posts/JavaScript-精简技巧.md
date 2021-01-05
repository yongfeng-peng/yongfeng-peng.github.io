---
title: JavaScript 精简技巧
date: 2020-09-14 14:47:30
categories:
- JS
tags: JavaScript 精简技巧
---
---

### JavaScript 精简技巧
#### 快速浮点数转整数
* 将浮点数转换为整数，可以使用Math.floor()、Math.ceil()或Math.round()。但是还有一种更快的方法可以使用|(位或运算符)将浮点数截断为整数。
```
console.log(23.2 | 0); // Result: 23

```
#### [JavaScript 浮点数陷阱及解法](https://github.com/nefe/number-precision)
* 数据展示类 如：1.4000000000000001 取整
```
parseFloat(1.4000000000000001.toPrecision(12)) === 1.4  // true
<!-- 简单封装 -->
function strip(num, precision = 12) {
  return +parseFloat(num.toPrecision(precision));
}
<!-- 为什么选择 12 做为默认精度？这是一个经验的选择，一般选12就能解决掉大部分0001和0009问题，而且大部分情况下也够用了，如果你需要更精确可以调高 -->
```
#### 数据运算类
* 对于运算类操作，如 +-*/，就不能使用 toPrecision 了。正确的做法是把小数转成整数后再运算
```
/**
  * 精确加法
*/
function add(num1, num2) {
  const num1Digits = (num1.toString().split('.')[1] || '').length;
  const num2Digits = (num2.toString().split('.')[1] || '').length;
  const baseNum = Math.pow(10, Math.max(num1Digits, num2Digits));
  return (num1 * baseNum + num2 * baseNum) / baseNum;
}
add(0.1, 0.2); // 0.3
```


