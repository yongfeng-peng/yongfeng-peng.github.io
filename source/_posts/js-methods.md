---
title: js常使用的函数总结
date: 2019-05-03 14:52:07
tags:
---

#### js常使用的工具函数
##### 序列号格式化输出
```
/**
  * @param {index} 索引
  * @des: 序列号格式化输出
  * @return: 格式化的字符串
*/
queueSerial (index) {
    let strZero = ['00', '0', ''];
    let curIndex = index + 1;
    return strZero[curIndex.toString().length - 1] + curIndex;
}
```

##### 查找数组中是否存在某个属性值，存在就返回
```
/**
  * @param {ist, attr, value}  数组，属性，属性值
  * @des: 查找数组中是否存在某个属性值，存在就返回
  * @return: Object
*/
function findOneByKey (list, attr, value) {
    let obj = {};
    let isExsit = false;
    for (let i = 0, len = list.length; i < len; i++) {
    if (list[i][attr] === value) {
        for (const _attr in list[i]) {
            obj[_attr] = list[i][_attr];
        }
        isExsit = true;
        break;
    }
    }
    return {isExsit, obj};
}
var list = [
    { name: 'aa', age: 12 },
    { name: 'bb', age: 17 },
    { name: 'aa', age: 18 }
];
```

#####  jQuery如何扩展自定义方法
```
(jQuery.fn.myMethod=function () {
    alert('myMethod');
})
// 或者：
(function ($) {
    $.fn.extend({
        myMethod : function () {
            alert('myMethod');
        }
    })
})(jQuery)
$("#div").myMethod();
```


##### 冒泡排序
```
/**
  * @param {arr}  排序数组
  * @des: 实现冒泡排序
  * @return: new arr
*/
function bubbling(arr) {
    var temp = 0; // 临时变量
    for(let i = 0; i < arr.length; i++) {
        for(let j = 0; j < arr.length - i; j++) {
            if(arr[j] > arr[j + 1]) {
                temp = arr[j + 1];
                arr[j + 1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}
```

##### 判断一个字符串中出现次数最多的字符，统计这个次数
```
let str = 'asdfssaaasasasasaa';
function findStrMax(str) {
    let resultObj = {};
    for (let i = 0; i < str.length; i++) {
        if (!resultObj[str.charAt(i)]) {
            resultObj[str.charAt(i)] = 1;
        } else {
            resultObj[str.charAt(i)]++;
        };
    };
    let strNum = ''; // 统计出现字符串最多的
    let totalNum = 0; // 统计字符串总的出现了多少次
    for(let i in resultObj) {
        if (resultObj[i] > totalNum) {
            totalNum = resultObj[i];
            strNum = i;
        };
    };
    console.log('出现次数最多的是：' + strNum + '出现' + totalNum +'次');
}
findStrMax(str);
```

##### 去掉一个数组的重复元素
* 计数排序变形
* 使用 Set
* 使用 WeakMap
```
// 方法1
let arr = [2, 3, 4, 4, 5, 2, 3, 6];
function getNewArr(arr) {
    let newArr = [];
    for (let i = 0; i < arr.length; i++) {
        if (newArr.indexOf(arr[i]) < 0) {
            newArr.push(arr[i]);
        };
    };
    console.log('新的数组' + newArr);
}
getNewArr(arr);
// 方法2
let newArr = arr.filter(function(ele, index, self) {
    return self.indexOf(ele) === index;
});
```

##### 去除字符串前后的空格
```
String.prototype.trim = function() {
    return this.replace(/^\s+|\s+$/g, '');
}
//或者 
function trim(string) {
    return string.replace(/^\s+|\s+$/g, '');
}
```

##### 判断一个数字是不是整数
```
function isInt(num) {
	return num % 1 === 0;
}
```

##### 获取浏览器的地址参数
```
function getUrlParams(url) {
    // var _href = window.location.href;
    let _href = 'http://www.runoob.com/jquery/misc-trim.html?channelid=12333&name=xiaoming&age=23';
    let _tmpArr = _href.split('?');
    let _paramArr = _tmpArr[1].split('&');
    let resultObj = {};
    let _arg = [];
    for (let i = 0; i < _paramArr.length; i++) {
        _arg = _paramArr[i].split('=');
        resultObj[_arg[0]] = _arg[1];
    };
    console.log(resultObj);
    return resultObj;
}
getUrlParams();
```

##### JS递归算法1+ 2+3.....100的和
```
function num(n) {
    if(n === 1) {
        return 1;       
    }
    return num(n - 1) + n;
}
num(100);
```

##### 找出一组数据中的奇数和偶数，偶数乘以2，并从小到大排列，奇数从大到小排列,最后合并数组
```
var arr = [4, 3, 8, 6, 2, 9, 1];
function findOddEven(arr) {
    let [oddArr, evenArr] = [[], []]; // oddArr奇数 evenArr偶数
    for(let i = 0, len = arr.length; i < len; i++) {
        if(arr[i] % 2 === 0) {
            evenArr.push(arr[i] * 2);
        } else {
            oddArr.push(arr[i]);
        }
    }
    oddArr.sort(function(a, b) {
        return b - a; // 从大到小
    });
    evenArr.sort(function(a, b) {
        return a - b; // 从小到大
    });
    console.log('奇数'+ oddArr);
    console.log('偶数'+ evenArr);
    return oddArr.concat(evenArr);
}
findOddEven(arr);
```

##### 根据json对象的某一属性对其进行排序
https://www.cnblogs.com/ygtq-island/p/6512728.html

```
var json = {
    'A': {
        'id': 1,
        'name': 'a'
    },
    'E': {
        'id': 5,
        'name': 'e'
    },
    'C': {
        'id': 3,
        'name': 'c'
    },
    'F': {
        'id': 6,
        'name': 'f'
    },
    'B': {
        'id': 2,
        'name': 'b'
    },
    'D': {
        'id': 4,
        'name': 'd'
    }
};
var obj = {name: 'lisa', age: 8, ace: 5, nbme: 'lisi'}; // 要排序的对象
/**
    * [objKeySort description] 排序的函数
    * @Author   pyf
    * @DateTime 2019-03-19T15:44:58+0800
    * @param    {[Object]}                 obj [需要排序的json对象]
    * @return   {[Object]}                     [排序后的对象]
*/
function objKeySort(obj) {
    // 	Object内置类的keys方法获取要排序对象的属性名，利用Array原型上的sort方法对获取的属性名进行排序，newKeyArr是一个数组
    var newKeyArr = Object.keys(obj).sort();
    var newObj = {}; // 存放排好序的键值对
    for(let i = 0, len = newKeyArr.length; i < len; i++) {
        newObj[newKeyArr[i]] = obj[newKeyArr[i]]; // 向新数组中按照排好的顺序依次增加键值对
    }
    return newObj; // 返回排好序的新对象
}
console.log(objKeySort(json));
```

##### js返回两个数组的差异值
```
// 普通算法
function diff(arr1, arr2) {
    var newArr = [];
    var arr3 = [];
    for (var i=0;i<arr1.length;i++) {
    if(arr2.indexOf(arr1[i]) === -1)   
        arr3.push(arr1[i]);
    }
    var arr4 = [];
    for (var j=0;j<arr2.length;j++) {
    if(arr1.indexOf(arr2[j]) === -1)
        arr4.push(arr2[j]);
    }
    newArr = arr3.concat(arr4);
    return newArr;
}

// 优雅
function diff(arr1, arr2) {  
    console.log(arr1.concat(arr2));
    return arr1.concat(arr2).filter(item => {
    return !arr1.includes(item)||!arr2.includes(item)
    });  
}

// 方式三
function diff(arr1, arr2) {  
    return arr1.concat(arr2).filter(item => !arr1.includes(item)||!arr2.includes(item));  
}  
diff([1, 2, 3, 5], [0, 2, 3, 4, 5]);
```

##### 某年某月的1号为星期几
```
var weekDay = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
weekDay[new Date(2019, 3, 1).getDay()];
```

##### 数据的排序 123454321 23456765432
```
// 方法1 
//01234543210
//先展示前面的   01234
//n：开始的数字    m:结束的数字
function num1(n, m) {
    for(var i = n; i < m; i++) {
        //再展示后面的 543210
        console.log(i);
        if (i === m-1) {
            num2(n, m)
        }
    }
}
function num2(n, m) {
    for(var i = m; i >= n; i--) {
        console.log(i)
    }
}
num1(2, 5)  //2345432
// 方法2
function num(n, m) {
    console.log(n);
    if (n < m) {
        num(n + 1, m);
        console.log(n);
    }
}
num(2, 5)  //2345432
```

##### 首字母大写 
```
/** 
* 首字母大写 
* @param str 字符串 
* @example heheHaha 
* @return {string} HeheHaha 
*/ 
function capitalizeFirstLetter (str) { 
    return str.charAt(0).toUpperCase() + str.slice(1);
}
```

##### 深拷贝
* 递归
* 判断类型
* 检查环（也叫循环引用）
* 需要忽略原型
理解深浅拷贝，<a href="https://www.cnblogs.com/echolun/p/7889848.html" style="color: blue;">需了解</a>
1. 基本类型--名值存储在栈内存中，例如let a=1
![栈内存](js-methods/stack.jpg)
2. 引用数据类型--名存在栈内存中，值存在于堆内存中，但是栈内存会提供一个引用的地址指向堆内存中的值
![堆内存](js-methods/heap.jpg)
```
function deepCopy(obj) {
    // 判断是否是简单数据类型，
    if (typeof obj == 'object') {
        //复杂数据类型
        var result = obj.constructor == Array ? [] : {};
        for(let i in obj) {
            result[i] = typeof obj[i] == 'object' ? deepCopy(obj[i]) : obj[i];
        }
    } else {
        //简单数据类型 直接 == 赋值
        var result = obj;
    }
    return result;
}
```

##### 将类数组转化为数组
```
function trigger() {
  var args = Array.prototype.slice.call(arguments);
  // ...
}
```

##### 使用 Array的 slice 进行数组复制（ 浅复制）
```
var items = [1, 2, 3];
// bad
var itemsCopy = [];
for (var i = 0; i < items.length; i++) {
  itemsCopy[i] = items[i];
}
// good slice返回的是一个新数组，和直接赋值区别
var itemsCopy = items.slice();
```


##### 使用 for-in 循环时需要避免对象从原型链上继承来的属性也被遍历出来，因此保险的做法是对 key 是否是对象自身的属性进行验证
```
// good
for (const key in foo) {
  if (Object.prototype.hasOwnProperty.call(foo, key)) {
    doSomething(key);
  }
}
```

##### if(a === 1 || a === 2 || a === 3){}推荐写法
```
[1, 2, 3].indexOf() || /^(1|2|3)$/.test()
// 第一种方式
if(a === 1 || a === 2 || a === 3) {
    // doThing
}
// 第二种方式，在数组判断是否存在此元素，不存在返回-1
if([1, 2, 3].indexOf(a) > -1) {
    // doThing
}
// 第三种方式1,2,3之间不能有空格
if (/^(1| 2|3)$/.test(a)) {
	// doThing
}
```

##### 三位数字分离与合并的相互转化
```
function numSplit(val) {
  if (val) {
	return val.toString().replace(/(\d)(?=(\d{3})+($|\.))/g, '$1,');
  }
}
console.log(numSplit(1003030)); // 1,003,030

// 1,333,112 ==> 1333112
function numMery (v) {
	return parseFloat(v.toString().replace(/,/g, ''));
}
console.log(numMery('1,333,112'));
```

###### 抽离出相同类型的数据
```
// es6
let _data = [
    {typeCnt: '独家特色', content: '24小时落货装卸分拨'},
    {typeCnt: '独家特色', content: '专属服务经理对接'},
    {typeCnt: "独家特色", content: "千元以下理赔24H赔付"},
    {typeCnt: "覆盖范围", content: "宁夏全境覆盖"},
    {typeCnt: "覆盖范围", content: "网络覆盖宁夏全境、乌海、巴彦淖尔和阿拉善盟，无盲区"},
    {typeCnt: "覆盖范围", content: "全省地级市自提点全覆盖"},
    {typeCnt: "定时必达", content: "省内地市中转次日达"},
    {typeCnt: "定时必达", content: "市配当日达"},
    {typeCnt: "超低价格", content: "银川三环以内配送40元/票起"},
    {typeCnt: "超低价格", content: "中转价格透明不议价"}
];
// ES6新的数据类型set
// set 集合，和数组很类似 
// 特点 数据有唯一性，可以用来去除重复数据
let typeCntGroup = new Set();
let result = [];
_data.forEach(item => {
typeCntGroup.add(item.typeCnt);
});
// ES6为Array增加了from函数用来将其他对象转换成数组。
// 当然，其他对象也是有要求，也不是所有的，可以将两种对象转换成数组。
// 1.部署了Iterator接口的对象，比如：Set，Map，Array。
// 2.类数组对象，什么叫类数组对象，就是一个对象必须有length属性，没有length，转出来的就是空数组。
typeCntGroup = Array.from(typeCntGroup);
for (let i = 0; i < typeCntGroup.length; i++) {
let _obj = {
    typeCnt: '',
    content: []
};
_obj.typeCnt = typeCntGroup[i];
for (let j = 0; j < _data.length; j++) {
    if (_data[j].typeCnt === typeCntGroup[i]) {
    _obj.content.push(_data[i].content);
    }
}
result.push(_obj);
}
console.log(result);
```

```
let _typeArr = []; // 枢纽展示类型
for (let i = 0; i < _data.length; i++) {
   // 将枢纽类型先存储下来
   if (_typeArr && _typeArr.indexOf(_data[i].typeCnt) < 0) {
     _typeArr.push(_data[i].typeCnt);
   }
 }
let _newArr = []; // 每个枢纽类型对应的content保存
for (let i = 0; i < _typeArr.length; i++) {
   let obj = {
    	typeCnt: '',
    	content: []
    };
    obj.typeCnt = _typeArr[i];
    for (let j = 0; j < _data.length; j++) {
        if (_typeArr[i] === _data[j].typeCnt) {
          obj.content.push(_data[j].content);
        }
    }
     _newArr.push(obj);
 }
```

```
// es5的处理
let _typeArr = []; // 枢纽展示类型
for (let i = 0; i < _data.length; i++) {
   // 将枢纽类型先存储下来
   if (_typeArr && _typeArr.indexOf(_data[i].typeCnt) < 0) {
     _typeArr.push(_data[i].typeCnt);
   }
 }
let _newArr = []; // 每个枢纽类型对应的content保存
for (let i = 0; i < _typeArr.length; i++) {
   let obj = {
    	typeCnt: '',
    	content: []
    };
    obj.typeCnt = _typeArr[i];
    for (let j = 0; j < _data.length; j++) {
        if (_typeArr[i] === _data[j].typeCnt) {
          obj.content.push(_data[j].content);
        }
    }
    _newArr.push(obj);
 }
```
##### 函数防抖和函数节流
1. 节流（一段时间执行一次之后，就不执行第二次）
**注意** 认为节流函数不是立刻执行的，而是在冷却时间末尾执行的（相当于施法有吟唱时间），那样说也是对的。
```
function throttle(fn, delay) {
    let canUse = true;
    return function() {
        if (canUse) {
            fn.apply(this, arguments);
            canUse = false;
            setTimeout(() => {
                canUse = true;
            }, delay)
        }
    }
}
 const throttled = throttle(() => console.log('hi'));
 throttled();
 throttled();
```
2. 防抖（一段时间会等，然后带着一起做了）
```
function debounce(fn, delay) {
    let timerId = null;
    return function() {
        const context = this;
        if (timerId) {
            window.clearTimeout(timerId);
        }
        timerId = setTimeout(() => {
            fn.apply(context, arguments);
            timerId = null;
        }, delay)
    }
}
const debounced = debounce( () => console.log('hi'))
debounced()
debounced()
```

##### 手写AJAX
```
    var request = new XMLHttpRequest();
    request.open('GET', '/a/b/c?name=ff', true);
    request.onreadystatechange = function () {
    if (request.readyState === 4 && request.status === 200) {
        console.log(request.responseText);
    }};
    request.send();
    // 简单一些可以如下
    var request = new XMLHttpRequest();
    request.open('GET', '/a/b/c?name=ff', true);
    request.onload = () => console.log(request.responseText);
    request.send();
```

##### 事件委托
```
// 简单版本
ul.addEventListener('click', function(e) {
    if (e.target.tagName.toLowerCase() === 'li') {
        fn() // 执行某个函数
    }
})
// bug 在于，如果用户点击的是 li 里面的 span，就没法触发 fn，这显然不对
// 优化版本    思路是点击 span 后，递归遍历 span 的祖先元素看其中有没有 ul 里面的 li
 function delegate(element, eventType, selector, fn) {
    element.addEventListener(eventType, e => {
        let el = e.target;
        while (!el.matches(selector)) {
            if (element === el) {
            el = null;
            break;
            }
            el = el.parentNode;
        }
        el && fn.call(el, e, el);
    })
    return element;
}
```

##### 用 mouse 事件写一个可拖曳的 div
<a href="https://jsbin.com/zolulovujo/edit?html,js,output" style="color: blue;">快速看到效果</a>
```
var dragFlag = false;
var position = null;
drag.addEventListener('mousedown', function(e) {
  dragFlag = true;
  position = [e.clientX, e.clientY];
})

// mousemove 与 mouseup 不能监听在元素上 快速拖到会有问题
document.addEventListener('mousemove', function(e) {
 if (dragFlag) {
    return;
  }
  const x = e.clientX;
  const y = e.clientY;
  const deltaX = x - position[0];
  const deltaY = y - position[1];
  // parseInt('')空转为 NaN
  // drag.style.left初始值为'0px'
  const left = parseInt(drag.style.left || 0, 10);
  const top = parseInt(drag.style.top || 0, 10);
  drag.style.left = left + deltaX + 'px';
  drag.style.top = top + deltaY + 'px';
  position= [x, y];
})

document.addEventListener('mouseup', function() {
    dragFlag = false;
})
```

##### filter方法
* 不会对空数组进行检测，会跳过那些空元素
```
let arr = [0, 3, 5];
arr[10] = 10;
arr.filter((val) => {
    return val === undefined;
});
// 初始化
let array = [0, 3, 5, undefined, undefined, null, 123];
array[10] = 10;
array.filter((val) => {
    return val === undefined;
});
```
##### join()
* javascript 在定义数组的时候允许最后一个元素后跟一个,(分号)
```
let str = [,,,].join(); // ,,
let str = [,,1,].join('.').length;
```
##### 普通函数与箭头函数的区别
* this指向的问题，会捕获其所在上下文的this值，作为自己的this值
* 箭头函数不绑定arguments，取而代之用reset参数解决
```
// 箭头函数
let foo = (...args) => {
  return args[0];
}
console.log(foo(1));
// 普通函数
fun() {
    console.log(argument[0]);
}
console.log(fun(1, 34));
```
* 箭头函数不能用作构造器，和new一起用就会抛出错误
* 箭头函数没有原型属性
```z
var foo = () => {};
console.log(foo.prototype) // undefined
```

##### js常见的自认为正确的问题
```
var array = [];
for(var i = 0; i <3; i++) {
    array.push(() => i);
console.log(array);
}
var newArray = array.map(el => el());
console.log(newArray); // [3, 3, 3]

```
* 解决问题为什么不是0,1,2
* var声明变量，三个箭头函数体中的每个i，都指向**相同的绑定**，所以它们在循环结束时返回相同的值3
* 解决方法1，把var改为let（let 声明一个具有块级作用域的变量，每个循环迭代创建一个新的绑定）
* 解决方式2，使用闭包
```
var array = [];
for(var i = 0; i <3; i++) {
    array[i] = (function(x) {
        return function() {
            return x;
        }
    })(i);
}
var newArray = array.map(el => el());
console.log(newArray); // [3, 3, 3]
```

##### 找出字符串中第一次只出现一次的字母
```
String.prototype.firstAppear = function () {
    let obj = {},
        len = this.length;
    for (let i = 0; i < len; i++) {
        if (obj[this[i]]) {
            obj[this[i]]++;
        } else {
            obj[this[i]] = 1;
        }
    }
    for (let prop in obj) {
        if (obj[prop] == 1) {
            return prop;
        }
    }
}
let str = 'abcdahfksdb';
console.log('result', str.firstAppear());
```

###### 获得滚动条的滚动距离
```
function getScrollOffset() {
    if (window.pageXOffset) {
        return {
            x: window.pageXOffset,
            y: window.pageYOffset
        }
    } else {
        return {
            x: document.body.scrollLeft + document.documentElement.scrollLeft,
            y: document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```

##### 获得视口的尺寸
* <a href="https://segmentfault.com/a/1190000019832694" style="color: blue;">To Learn More...</a>
```
```

