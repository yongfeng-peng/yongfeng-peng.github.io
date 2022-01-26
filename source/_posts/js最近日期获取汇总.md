---
title: js最近日期获取汇总
date: 2022-01-26 11:18:21
categories:
- JS
tags:
---

### 常用日期汇总
```
  * 获取近半年日期
  function halfYear() {
    // 先获取当前时间
    let curDate = (new Date()).getTime();
    // 将半年的时间单位换算成毫秒
    let halfYear = 365 / 2 * 24 * 3600 * 1000;
    // 半年前的时间（毫秒单位）
    let pastResult = curDate - halfYear;
    // 日期函数，定义起点为半年前
    let pastDate = new Date(pastResult);
    let pastYear = pastDate.getFullYear();
    let pastMonth = pastDate.getMonth() + 1;
    pastMonth = pastMonth.toString().padStart(2, 0);
    let pastDay = pastDate.getDate().toString().padStart(2, 0);
    let endDate = pastYear + '-' + pastMonth + '-' + pastDay;
    return endDate;
  }
```
```
  * 获取近一年日期
  function reactYear() {
    let nowDate = new Date();
    let dates = new Date(nowDate);
    dates.setDate(dates.getDate() - 365);
    let year = dates.getFullYear();
    let month = dates.getMonth() + 1;
    month = month.toString().padStart(2, 0);
    let strDate = dates.getDate().toString().padStart(2, 0);
    let currentDate = year + '-' + month + '-' + strDate;
    return currentDate;
  }
```
```
  * 获取当前日期
  function getCurDate(type) {
    const date = new Date();
    // date.setTime(date.getTime() + 24 * 60 * 60 * 1000);
    date.setTime(date.getTime());
    let year = date.getFullYear()
    let month = date.getMonth() + 1
    month = month.toString().padStart(2, 0);
    let strDate = date.getDate().toString().padStart(2, 0);
    let hour = date.getHours().toString().padStart(2, 0);
    let minutes = date.getMinutes().toString().padStart(2, 0);
    let seconds = date.getSeconds().toString().padStart(2, 0);
    let curDate = year + '-' + month + '-' + strDate;
    if(type) {
      return `${year}-${month}-${strDate} ${hour}:${minutes}:${seconds}`;
    }
    return curDate;
  }
```

```
  * 最近三月
  function beforeThree() {
    const dates = new Date()
    dates.setMonth(dates.getMonth() - 3);
    let pastMonth = dates.getMonth() + 1;
    let pastDay = dates.getDate();
    pastMonth = pastMonth.toString().padStart(2, 0);
    pastDay = pastDay.toString().padStart(2, 0);
    const endDate = dates.getFullYear() + '-' + pastMonth + '-' + pastDay;
    return endDate;
  }
```

```
  * 最近一月
  function oneMonth() {
    const dates = new Date();
    dates.setMonth(dates.getMonth() - 1);
    let pastMonth = dates.getMonth() + 1;
    let pastDay = dates.getDate();
    pastMonth = pastMonth.toString().padStart(2, 0);
    pastDay = pastDay.toString().padStart(2, 0);
    const endDate = dates.getFullYear() + '-' + pastMonth + '-' + pastDay;
    return endDate
  }
```

```
  * 某年某月最后一天
  function getLastDay(year, month) {
    //获取本年本月的第一天日期
    var date = new Date(year,month+1,'01');
    //设置日期
    date.setDate(1);
    //设置月份
    date.setMonth(date.getMonth());
    //获取本月的最后一天
    let cdate = new Date(date.getTime() - 1000*60*60*24);
    //返回结果
    // return cdate.getDate();
    return `${cdate.getFullYear()}-${cdate.getMonth()+1}-${cdate.getDate()}`;
  }
```

```
  * 当月
  function getMonthStartDate() {
    const dates = new Date();
    dates.setMonth(dates.getMonth());
    let pastMonth = dates.getMonth() + 1;
    pastMonth = pastMonth.toString().padStart(2, 0);
    const endDate = dates.getFullYear() + '-' + pastMonth + '-' + '01';
    return endDate
  }
```

```
  * 获取某月多少天
  const _temp = '2021-06-12';
  const month = _temp.substr(_temp.lastIndexOf("-") + 1, 2);
  const year = _temp.substr(0, _temp.lastIndexOf("-"));
  // console.log(1111, year, month, new Date(year, month, 0).getDate())
  // 获取月份多少天
  const day = new Date(year, month, 0).getDate();
```

```
  * 获取当年当月第一天
  getYearStartDate() {
    const dates = new Date();
    dates.setMonth(dates.getMonth());
    const endDate = dates.getFullYear() + "-" + "01" + "-" + "01";
    return endDate;
  }
```

```
  // utils.js
  const globalFun =  {
    halfYear, reactYear, getCurDate, beforeThree, oneMonth,getLastDay, checkFull, getMonthStartDate,
  };
  export default globalFun;

  // 全局方法挂载 main.js
  import globalFun from "@/utils/utils.js";
  Object.keys(globalFun).forEach((key) => {
    Vue.prototype[key] = globalFun[key];
  })
```
