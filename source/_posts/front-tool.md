---
title: front-tool前端开发工具
date: 2019-07-20 14:01:02
categories:
- ide
tags:
---

#### 开发工具插件
##### VScode常有插件
* <a href="https://juejin.im/post/5d300d5ff265da1b7004e0c1">To Learn More...</a>
* 安装javascript console utils （crtl + shift + l）快捷console语句
* 安装Path Autocomplete 路径自动补全

##### VScode快捷键
* <a href="https://juejin.im/post/5d34fdfff265da1b897b0c8d">To Learn More...</a>
* ctrl + shift + f(command + shift + f)项目目录中，搜索任何匹配的文本
* 合并行 Control + J
**左侧可以单击小箭头，出现第二框，即替换的内容**

* 为 tabs 设置强调色（Material Theme）（Command + K 再 Command + T）
    * 打开的命令面板(Ctrl(Command) + Shift + P)，选择 Material Theme: Set accent color并从列表中选择一个颜色，它将更改选项卡的下划线颜色

* 进程资源管理器
    * 在VsCode 中按Ctrl + Alt + Delete可以打开该任务管理器

* Expand Bracket Selection
    * 打开键盘快捷键(Ctrl + Shift + P 或 command + Shift + p)，搜索 Expand Bracket Selection
    * 自动选择整个块，从开始的大括号到结束(比如选择if/else)

* 重新打开关闭的编辑页面（Windows: Ctrl + Shift + T Mac: command + Shift + T）

* 通过匹配文本打开文件
    * Windows: Ctrl + T Mac: command + Tconsole.log();
    
* 集成终端
    * Windows: Ctrl + Mac: control +
    * 通过 **Ctrl + `**可以打开或关闭终端

* 查看正在运行的插件
    *  (Ctrl + Shift + P)并输入 Show running extensions 

* 重新加载
    * Windows: Ctrl + Alt + R Mac: Control + Option + R

* 移动tabs栏
    * Mac：Command + Option + 左右键 Windows:Ctrl + Alt + 左右键头

* 选择左侧或者右侧所有的内容
    * Mac：Command + Shift + 左右键 Windows:Ctrl + Shift + Home/End

* 删除上一个单词
    * Mac：option + delete Windows: Ctrl + Backspace

* 逐个选择文本
    * Mac：option + shift + 左右键 Windows: Ctrl + shift + 左右键

* 重复行
    * Mac：option + shift + 上下键 Windows: Alt + shift + 上下键

* 移至文件的开头和结尾
    * Mac：Command + 上下键 Windows: Ctrl + Home/End

* 批量替换当前文件中所有匹配的文本
    * Ctrl + F2 (Mac: Command + F2)一次改所有出现的文本

* 向上/向下移动一行
    * Mac：option + 上下键 Windows: Alt + 上下键

* 删除一行
    * Mac：Command + X / Command + shift + k Windows: Crtl + X /Ctrl + shift + K
    
* 复制光标向上或者向上批量添加内容
    * Mac：Command + option + 上下箭头  Windows: Ctrl + Alt + 上下箭头
##### vscode注释插件推荐
* Better Comments：特点是可以改变注释的颜色，通过不同颜色来表示不同情况
```
 文件顶部的注释，包括描述、作者、日期、最后编辑时间，最后编辑人
 
  ```typescript
  /*
   * @Description: app im菜单配置列表中的表格模块
   * @Author: maoyuyu
   * @Date: 2019-08-05 10:31:12
   * @LastEditTime: 2019-08-14 17:08:33
   * @LastEditors: Please set LastEditors
   */
  ```
 
  > **文件头部添加注释快捷键**：`window`：`ctrl+alt+i`,`mac`：`ctrl+cmd+i`
  >
  > **在光标处添加函数注释**：`window`：`ctrl+alt+t`,`mac`：`ctrl+cmd+t`
  >
  > **vscode setting 配置如下**
 
  ```json
  "fileheader.configObj": {
    "autoAdd": false, // 自动添加头部注释开启才能自动添加
  },
  "fileheader.customMade": {
    "Author":"[you name]",
    "Date": "Do not edit", // 文件创建时间(不变)
    "LastEditors": "[you name]", // 文件最后编辑者
    "LastEditTime": "Do not edit", // 文件最后编辑时间
    "Description":""
  },
  "fileheader.cursorMode": {
    "Author":"[you name]",
    "description": "", 
    "param": "", 
    "return":""
  }
  ```
```
* koroFileHeader：特点是可以在文件头部快捷添加注释和函数注释
```
  ！ ， 红色注释
  ? , 蓝色注释
  // , 灰色删除线注释
  todo ,橘红色注释
  * , 浅绿色注释
  * setting.json配置
  "better-comments.tags": [
    {
      "tag": "!",
      "color": "#FF2D00",
      "strikethrough": false,
      "backgroundColor": "transparent"
    },
    {
      "tag": "?",
      "color": "#3498DB",
      "strikethrough": false,
      "backgroundColor": "transparent"
    },
    {
      "tag": "//",
      "color": "#474747",
      "strikethrough": true,
      "backgroundColor": "transparent"
    },
    {
      "tag": "todo",
      "color": "#FF8C00",
      "strikethrough": false,
      "backgroundColor": "transparent"
    },
    {
      "tag": "*",
      "color": "#98C379",
      "strikethrough": false,
      "backgroundColor": "transparent"
    }
  ]
```
