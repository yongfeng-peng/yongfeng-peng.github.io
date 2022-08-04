---
title: web安全
date: 2022-08-03 16:37:34
tags:
---

### web安全
#### 网站或移动端
* 硬件
  * 服务器---》运维的 硬件 软件
* 软件或者程序
  * 后台 java php python ...编程都涉及到程序安全
  * 前端 与后台交互 容易出现安全隐患
    * 加密 校验 程序的抗压力

#### xss攻击（跨站脚本攻击）
* web页面
  * 包含html js文件 嵌入可执行代码(js) 
  * 网络传输的时候，js文件可下载的或通过ajax提交
* xss危害
  * 用户提交的数据，未经处理之前 ===》 js obj.value
  * 强行改变程序的执行方式
* xss解决
  * 运维=>设置只读
  * 通过后台程序，写一套监测程序，监测文件的大小改变，报警更换文件
  * 校验用户输入信息 正则验证合法性
  * 对特殊符号 ！、？、@... 进行过滤

#### 跨站请求伪造(csrf攻击)
* 用户和后台交互数据的时候，交互的页面是伪造页面，所有的用户信息将被泄露
* csrf解决
  * token 添加二次校验
  * 验证码 手机号 微信验证（保证验证信息 是自己项目发的）

#### sql注入
* 一般发生在注册、评论、添加商品...凡是有输入的地方，就有可能发生sql注入
* 原理
  * or 1 == 1 or 2 > 3
  * and select * from user where uName = $uName and uPwd = $uPwd
                                * uName = '' or 1 == 1 and uPwd = '' or 1 == 1
* 危害：任意账户 任意登录 任意操作
* 避免：前端 
--- 添加正则校验（特殊符号）
--- 屏蔽敏感词汇（过滤程序）
--- 缩减用户权限（取最小）
--- sql语句（有工具可以检测漏洞）

#### 接口加密
* url?keyword=va11 & id=3 & name=xx & pwd=1333 --- get传输、post传输有可能 --- 数据有可能会被劫持---》抓包
* js原生
```
let url = "https://www.baidu.com"
let params = "?keyword="+val
params+="&id=3"
params+="&name"+"xxx"
params+="&pwd"+"123"
```
* md5() --- 加密方式 --- "?keyword="+md5(val) --- 值加密
* 路径加密 url = md5(url+params)
* 前端后端都需要加密 ---- 加密是双向的
* vue 加密
  * 加密软件对象 npm install bcryptjs --- 一般这种加密包，加密方式不止一种
  * 对象.标准.加密方法(数据, 密匙, 参数)
```
npm install bcryptjs
import bcrypt from 'bcryptjs'
var salt = bcrypt.genSaltSync(12);    //定义密码加密的计算强度,默认10
var hash = bcrypt.hashSync(明文密码, salt);    //变量hash就是加密后的密码
```
