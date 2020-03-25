---
title: npm与yarn
date: 2019-07-08 13:59:28
tags:
---

#### npm与yarn
* 对于已经安装过的包
    * yarn确实比npm快，而且节省空间。
* 对于从来没有安装过的包
    * yarn会节省空间，但是会比npm安装速度慢一些（yarn要40多s，而npm时间10多s）
* yarn比npm更可靠 （在npm5.0之前，因为没有package-lock.json文件，安装都是依赖package.json文件的。而package.json中对于模块的版本配置都是不确定的）
* yarn比npm更安全（yarn比npm检查更加严格）
* yarn和npm的命令
    * <a href="https://www.jeffjade.com/2017/12/30/135-npm-vs-yarn-detial-memo/" style="color: blue;">想了解more跳转</a>
* yarn和npm安装或者升级包的时候，都可以指定对应的版本
```
npm install [package]@[version]
npm install [package]@[tag]
yarn add [package]@[version]
yarn add [package]@[tag]
```
* yarn和npm独有的命令