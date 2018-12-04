---
title: CSS3 新特性
date: 2016-03-02 18:32:09
categories: CSS
tags: 前端
---

 [1]icon图标的变化

<font color="red" size="4">1、transform：rotate（360deg） scale(1.2);</font>
>[意思是让icon图标旋转360度]  [ 在x和y轴上的缩放]
>会有浏览器兼容问题，一般解决办法就是加前缀；
>-ms-transform:........;
>-moz-transform:....;
>-webkit-transform:.......;
>-o-transform:.....;

2、<font color="red" size="4">transition:0.2s ease-out;</font>
>a.让icon图标从小到大慢慢旋转变大，并且设置旋转地速度；
>兼容性问题同上解决，在其前加那些浏览器内核.
>b.让鼠标经过时的变化是 渐变的 ，不是突然一下子就变大或变小，一般在.button的时候就定义， -moz-transition:0.4s ease; 还有ease-in,ease-inout,ease-out这三种；


3、<font color="red" size="4">让边框透明度可调</font>
>border:2px solid rgba(255,255,255,0.8);
>hover:border:2px dolid rgba(255,255,255,1);


4.<font color="red" size="4">box-sizing</font>

>让盒子的宽显示时不再加上border宽和padding值，将他俩的值都算入内是width的值