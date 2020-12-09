---
title: google扩展程序
date: 2020-12-03 11:41:43
categories:
- tool
tags: tool

---
---

#### google扩展程序安装
##### vue-devtools工具的安装
  * 创建一个新的文件夹，名为vue-devtools
  * sudo npm install vue-devtools
  * 进入vender文件夹，打开manifest.json,进行编辑
  * persistent修改成true
  * 打开扩展程序,把刚刚修改的vender文件夹拖拽进来就可以了(记得开启开发者模式)

##### react-devtools工具的安装
* git clone https://github.com/facebook/react-devtools
* cd react-devtools
* yarn install
* yarn build:extension(具体命令，在package.json文件中的script下查找以下命令)
* 打开扩展程序,把生成的shells/chrome/build/unpacked文件夹拖拽进来就可以了(记得开启开发者模式)

##### redux-devtools工具的安装
* git clone https://github.com/zalmoxisus/redux-devtools-extension.git
* npm i && npm run build:extension (npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver)
* 打开扩展程序,把生成的./build/extension文件夹添加到压缩程序(记得开启开发者模式)

##### 其他好用扩展程序
* WEB前端助手(FeHelper)
  * 一款国产的、超级实用的前端开发工具合集，之前扩展迷也用专文介绍过这款插件。
  * WEB前端助手(FeHelper)包含多个独立小应用，比如：Json工具、代码美化、代码压缩、二维码、Postman、markdown、网页油猴、便签笔记、信息加密与解密、随机密码生成、Crontab等等。
  * <a href="https://www.extfans.com/web-development/pkgccpejnmalmdinmhkkfafefagiiiad/" style="color: blue;">down...</a>
