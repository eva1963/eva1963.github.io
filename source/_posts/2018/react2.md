---
title: react的周期函数
date: 2018-2-26 21:06:24
categories: react
tags: react,周期函数
---

### 钩子函数（生命周期函数）
@(react)

描述一个组件或者程序从创建到销毁的过程，我们可以在过程当中基于钩子函数完成一些自己的操作，例如，在第一次渲染完成做什么或者在第二次即将重新渲染之前做什么事儿

#### 【基本流程】

constructor创建一个组件

componentWillMount  第一次渲染之前

render 第一次渲染

componentDidMount  第一次渲染之后



#### 【修改流程】

>  当组件的状态数据发生变（setState）化或者传递给组件的属性发生改变（重新调用组件传递不同的属性）都会引发render重新执行渲染（渲染也是差异渲染））

shouldComponentUpdate  是否允许组件重新渲染

componentWillUpdate   重新渲染之前

render第二次及以后重新渲染

componentDidUpdate 重新渲染之后

componentWillReceiveProps：父组件把递归子组件的属性发生改变 后触发的钩子函数

#### 【卸载】

> 原有渲染内容是不消失的，只是以后不能基于数据改变视图了

componentWillUnmount：卸载组件之前
![Alt text](./2AD321299AE1F7F0E2246279CF97B71A.png)

