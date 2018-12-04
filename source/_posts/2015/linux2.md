---
title: mac系统如何显示隐藏文件
date: 2015-07-11 01:10:46
categories: LINUX
tags: linux
---
### mac系统如何显示隐藏文件|文件已损坏--修改为任何来源


苹果Mac OS X操作系统下，隐藏文件是否显示有很多种设置方法，最简单的要算在Mac终端输入命令。显示/隐藏Mac隐藏文件命令如下(注意其中的空格并且区分大小写)：
显示Mac隐藏文件的命令：defaults write com.apple.finder AppleShowAllFiles -bool true
隐藏Mac隐藏文件的命令：defaults write com.apple.finder AppleShowAllFiles -bool false
或者
显示Mac隐藏文件的命令：defaults write com.apple.finder AppleShowAllFiles  YES
隐藏Mac隐藏文件的命令：defaults write com.apple.finder AppleShowAllFiles  NO


文件已损坏--修改为任何来源
高能预警！！！！！！！！(专门为不仔细看文章的准备的。。。。。。。没办法，只能放这么大了，要不老有人在评论里面问)
如果没有这个选项的话（macOS Sierra 10.12）,打开终端，执行sudo spctl --master-disable即可

