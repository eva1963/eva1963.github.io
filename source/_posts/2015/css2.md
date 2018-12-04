---
title: 自定义滚动条的样式
date: 2015-12-02 01:04:23
categories: CSS
tags: css
---
在实际项目中我们总会有些时候需要修改浏览器默认的滚动条的样式，因为每次遇到都要再查一遍，感觉很麻烦，所以就把自定义的样式放在这里，以备下次查阅，提高项目效率；

/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/  
```css
::-webkit-scrollbar  {  
    width: 16px;  /*滚动条宽度*/
    height: 16px;  /*滚动条高度*/
}  
```

/*定义滚动条轨道 内阴影+圆角*/ 
```css
::-webkit-scrollbar-track  {
-webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);  
border-radius: 10px;  /*滚动条的背景区域的圆角*/
background-color: red;/*滚动条的背景颜色*/  
}  
```

/*定义滑块 内阴影+圆角*/  
```css
::-webkit-scrollbar-thumb {  
    border-radius: 10px;  /*滚动条的圆角*/
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);  
    background-color: green;  /*滚动条的背景颜色*/
}
```
