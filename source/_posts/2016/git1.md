---
title: 常用命令git&& Linux
date: 2016-07-15 11:15:56
categories: LINUX
tags: linux,git
---
window操作系统：DOS窗口和DOS命令
Linux服务器操作系统：Linux命令（Mac终端使用的也是）

常用的lunix命令：

    ls -l/-a: //查看当前目录结构(-a是可以看到所有的，包括隐藏的)
    cd xxx	//进入某个目录
    cd /	//返回根目录
    cd ../	//返回上级目录
    cd ./ 	//当前目录
    clear 	//清屏
    mkdir	//创建空文件夹
    touch	//创建空文件
    vi	项文件中插入或者管理一些内容
    	i	//=>进入到插入模式
    	ESC+ ：wq   //=> 保存并退出\
    	ESC+q!    //=>强制退出，不保存
    	
    echo 	//向指定的文件中输入内容
    	echo XXX
    	echo XXX > XXX.txt
    	echo XXX >> XXX.txt
    cat   //查看内容
    rm //=>删除文件	//一旦删除不可还原	
    	-r	递归删除
    	-f	强制删除
    man XXX
    	gg回到顶部
    	G回到底部
    	/XXX是查找XXX
    	n	下一个
    	N	上一个
    whitch ls 查找命令
    find ./ -type f -name *.md
    cp 文件1 文件2；
    cp -rv 文件夹1 文件夹2；
    echo $PATH

常用的git操作命令：
 1.如果是第一次使用git，生成历史版本的时候，需要提供身份认证
```javascript
//  只需要在本地GIT全局环境下配置一些信息即可
  $ git config -l
  $ git config --global user.name 'xxx'
  $ git config --global user.eamil 'xxx'
```
**git init**
**git rm —cached XXX.XX**	//删除暂存区的某个文件的提交

**git rm —cached . -f**	// 删除暂存区的所有提交

**git checkout XXX.XX** 	

//把提交到暂存区的东西回溯到工作区，覆盖掉当前暂存区对本文件的修改,保持工作区和暂存区一致

git log  查看历史版本

**git reflog**  查看所有提交记录，包括回滚的版本



**git checkout .**	把当前暂存区回滚到工作区，一旦回滚工作区内容将无法恢复

**git reset HEAD .**	坝上一个暂存区内容回滚到工作区

**git reset —hard 版本号**	强制吧工作区和暂存区都变成回退后的版本了

**history > XXX.txt**

**npm root -g**





