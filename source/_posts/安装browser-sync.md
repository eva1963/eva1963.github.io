---
title: 安装browser-sync
date: 2017-03-02 18:30:48
categories: 插件模块
tags: 插件
---
browser-sync是一款很好用的通过在同一局域网给出的网址来从不同的终端来访问网页的技术，他极大地解决了我们由于网页需要在手机或者ipad上浏览而不得不安装各种环境或者软件的窘况，现在我来讲一下安装这项牛逼插件的过程:

1、首先，你要在自己的电脑上保证有node.js环境，然后我们使用终端用node.js的包管理器npm来进行接下来的事情；

2、执行npm install -g browser-sync;

3、接着执行browser-sync      这一步我出现了很大的问题，因为电脑上没有安装x-code工具，但我们这里先执行一下： sudo npm install npm -g，来保障我们安装的node版本是最新的，然后检查一下自己的版本，执行命令为 npm -v,node -v;

4、接下来我们就正式开始项目的创建了，现在桌面上创建一个文件夹：执行
mkdir liuhuan-browsersync(这里是文件名)；

5、执行cd liuhuan-browsersync;进入文件夹里

6、执行npm init；这里是创建版本库信息，一路回车即可；

7、执行npm  install  browser-sync  —save-dev;这一步是在文件夹里面安装browser-sync，对的，需要在node里安装一次，文件夹里安装一次；

8、执行 ls; 查看是否有node_modules这一项；

9、执行 ls node_modules,查看是否有browser-sync这一文件夹；有则成功

在项目中使用：

将写好的项目文件夹直接复制到liuhuan-browsersync文件夹下或者直接在liuhuan-browsersync文件夹中创建一个 新的项目文件夹,
然后打开终端cd 文件夹名这里是 cd desktop/liuhuan-browsersync,
然后执行
browser-sync start —server  web-app(项目文件夹名)  —files “web-app/*"
