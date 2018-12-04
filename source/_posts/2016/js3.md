---
title: call、apply和bind三者之前的区别
date: 2016-12-03 14:36:28
categories: js
tags: js
---


相信大家在做项目的时候或者刚开始学习JS的时候都或多或少的接触过call，本媛也是，但是一直停留在应用层，从来没有真正的研究过其原理，正好得空，本媛就查了一些资料然后根据自己的理解整理笔记放到网站，希望以后自己想不起或者需要用到的时候能够方便查阅：

> 用来改变某个函数中this关键字指向的

#### call
`[Fn].call([this],[param]...)`

fn.call 当前实例fn通过原型链的查找机制找到Function.prototype上的call方法
fn.call();把找到的call方法执行
+ 当call方法执行的时候，内部处理了一些事情
- 首先把要操作的函数中的this关键字变为call方法第一个传进去的实参值
- 把call方法第二个及第二个以后的实参获取到
- 把要操作的函数执行，并且把第二个以后传递进来的实参传给函数

#### call细节问题：
- 非严格模式下，如果参数不传，或者第一个传递的是null/undefined，this都指向window
- 严格模式下，第一个参数this是谁就指向谁，包括null，undefined，不传也是undefined

#### apply
+ 和call基本上一模一样，唯一区别在于传参方式：

```javascript
fn.call(obj,10,20);
fn.apply(obj,[10,20])
```
apply把需要传递的参数放进一个数组或者类数组中传递，虽然写的是一个数组，但是也相当与给fn一个个的传参
#### bind
>  语法和call一模一样，唯一区别在于call是立即执行，bind是等待执行，而且bind不兼容IE6~8
