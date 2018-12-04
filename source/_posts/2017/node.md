---
title: node基础知识
date: 2017-08-11 11:55:10
categories: NODE
tags: node,js
---

#### NODE基础概念：
1. node并不是一门语言，它是一个工具或者环境
2. 基于V8引擎（webkit）渲染和解析js的
3. 单线程
4. 无阻塞I/O操作
5. 事件驱动
> **之所以把node称之为服务器端语言，是因为node给予js操作服务器的能力：我们在服务器端安装node，只用js完成服务器端需要处理的一些事情，最后把写好的js代码交给node环境运行即可**

2. 在node环境中把js代码执行
- REPL命令（Read-Evaluate-Print-Loop：输入-求值-输出-循环）
- 基于node XXX.js 命令执行
- 基于编辑器这类工具直接执行

> **基于node命令执行，我们需要先找到当前文件所在的文件夹，然后在这个文件下打开DOS窗口在窗口中执行node XXX.js 这样就相当于在node环境下把js文件中代码执行了**

#### 扫盲命令
`ping www.baidu.com`  测试网速
`ifconfig -a`查看当前电脑的物理地址/ip/子网掩码/DNS等信息
`pwd`查看当前路劲
`cd /`返回根目录
`mkdir`创建文件夹
`touch XXX.js`创建文件
`rm`移除文件
`rmdir`移除文件夹
`cat`查看文件
`cp`拷贝
iterm:`⌘+d` 水平切分，`⌘+Shift+d` 垂直切分


#### NPM模块管理
安装完成node后，基本自带npm包管理器；
- 可以基于npm/yarn/bower;
1. npm系在的资源都是在https://npmjs.com下载的;
- `npm install xxx`:把资源或者第三方模块下载到当前目录下；
- `npm install xxx -g`把资源或者第三方模块安装到全局环境下，目的是以后可以基于命令操作一些事情；
- `npm uninsrall XXX/npm uninstall xxx -g`卸载

2. 解决下载慢的问题
- 基于nrm切换到国内下载源（一般是淘宝镜像）
  -  `npm install nrm -g`
  -  `nrm ls`
  -  `nrm use XXX`
- 可以基于yarn来安装管理
  - 首先还是需要先安装yarn，然后基于yarn来那住哪个我们需要的模块(只能安装到本地，不能安装到全局)
  - `npm install yarn -g`
  - `yarn add XX`
  - `yarn remove XX`
- 基于cnpm淘宝镜像来处理



3. 解决安装版本的问题
>首先查看当前模块的历史版本信息
>`npm view jquery > jquery.version.json`:把当前模块的历史信息输出到具体的某个文件中
> 安装指定的版本模块
>`yarn add jquery@1.11.3`
>`npm install jquery@1.11.3`


查看npm全局安装位置：
`npm root -g`   
**/Users/Eva/.nvm/versions/node/v8.11.1/lib/node_modules**



课后拓展：
bower 是从github下载安装研究一下使用？
全局安装： less/babel-cli/...






