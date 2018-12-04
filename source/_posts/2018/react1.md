---
title: JSX语法
date: 2018-02-24 21:07:52
categories: react
tags: react,node,js,JSX
---
【渐进式框架】

一种最流行的框架设计思想，一般框架中包含很多内容，这样导致框架的体积过于臃肿，会拖慢加载的速度，真实项目中，我们使用一个框架不一定用到所有的功能，此时我们应该把框架的功能拆分，用户想用什么，让其自己自由组合即可；

全家桶:渐进式框架N多部分组合

VUE全家桶：vue-cli/vue/vue-touter/vuex/axios/vue element(vant)

REACT全家桶：react/react-dom/react-router/redux/react-redux/axios/ant/dva/saga/mobx...

1. **react**：react框架的可信部分，提供了Component类可以供我们进行组件开发，提供钩子函数（生命周期函数：**所有的生命周期函数都是基于回调函数完成的**)
2. **react-dom**：把JSX语法（react独有的语法）渲染为真实DOM（能够放到页面中展示的结构都叫做真实DOM）的组件；react中特点：我们所有的结构和js都在js文件中编写的


3. **ReactDOM.render([JXS],[content],[callback])**：把JXS元素渲染到页面中

- JXS：react虚拟DOM元素
- content： 容器，我们想把元素放到页面中的哪个容器中
- callBack: 当把内容放到页面中呈现触发的回调函数

### **JXS**(javascript XML(HTML)): react独有的语法

> 和我们之前自己拼接的HTML类似，都是吧HTML结构代码和JS代码或者数据混合在一起，**但是它不是字符串**

1. 不建议我们把JXS直接渲染到body当中，一般我们都放在index.html的#root中
2. 在JSX中出现的`{}`是存放JS的，但是要求JS代码执行完成需要有返回结果
   - 里面不能是`对象/函数`，可以是自执行函数，数组可以但是数组里面不能放对象
   - 可以是一个基本类型的值（布尔类型的什么都不显示，null,undefined也是JSX元素，代表的是空）
   - 支持三元运算符
3. 循环数组创建JSX元素（一般都基于数组的MAP方法完成迭代），需要给创建的元素设置唯一的KEY值（当前本次循环内唯一即可）
4. 只能出现一个根元素
5. 给元素设置样式类用的是className而不是class
6. style中不能直接的写样式字符串，需要基于一个样式对象来遍历赋值

```javascript
let data = [{
        name: 'eva',
        age: 12
    }],
    root = document.querySelector('#root');
ReactDOM.render( < div id = 'box' className='box1' style={{color: #fff;fontSize: 12px;}}>
    return <ul> {
        data.map((item, index) => {
            return <li key = { index } >
                <span > { item.name } < /span> 
                <span > { item.age } < /span> 
                </li > ;
        })
    } 
    </ul> 
    < / div > , root, () => {
        let box = document.querySelector('#box');
        console.log(box.innerHTML);
    })
```

