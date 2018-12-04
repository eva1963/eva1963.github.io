---
title: DOM的回流和重绘
date: 2016-05-01 01:17:00
categories: JS
tags: js,dom
---
1. 计算DOM结构（DOM  tree）
2. 加载CSS
3. 生成渲染树（render tree）渲染书是和样式相关的
4. 浏览器基于GPU开始按照render tree画页面

`重绘`：当某一个元素样式更改（位置没关，只是样式修改）浏览器会重新渲染这个元素
```css
box.style.color = 'red';
//.......还有一些其他代码
box.style.background = 'red';
上面的操作出发了两次重绘，性能上有所消耗，真实项目中为了优化这个我们最好一次行的把修改的样式搞定，例如：
.xxx {
color: red;
fontSize: 14px;
}
```

`回流`：当DOM的结构或者位置发生改变（删除，增加元素，改变元素位置，改变大小）都会引发回流；所谓回流，就是浏览器抛弃原有的计算的结构和样式，从心进行DOM tree 或者render tree ，非常耗性能；
案例：
```javascript
//非常耗性能的fnag
data.forEach(()=>{
    let curLi = document.createElement('li');
    curLi.innerHTML = `<a href="#">
                <img src="${img}" alt=""/>
                <p>${title}</p>
                <span>￥${price}</span>
                <span>时间：${time}</span>
                <span>热度：${hot}</span>
            </a>`;
    document.querySelector('.list').appendChild(curLi);
});

//解决方案：
// 文档碎片：虚拟内存中开辟的一个容器，当我们把需要的元素都创建好了，而且都添加到文档碎片后在统一把文档碎片放到页面中，这样就只会引发一次回流；

let frg = document.createDocumentFragment();

data.forEach(()=>{
   let curLi = document.createElement('li');
   curLi.innerHTML = `<a href="#">
                <img src="${img}" alt=""/>
                <p>${title}</p>
                <span>￥${price}</span>
                <span>时间：${time}</span>
                <span>热度：${hot}</span>
            </a>`;
   frg.appendChild(curLi);
});
document.querySelector('.list').appendChild(frg);
frg = null;
```
