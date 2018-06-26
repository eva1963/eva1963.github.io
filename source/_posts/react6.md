---
title: react-redux
date: 2018-03-09 02:04:04
categories: react
tags: react,react-redux
---

>  它是将redux进一步封装，是操作起来适配react项目，让redux操作更简洁；

- store文件夹原封不动，和redux的写法一模一样
- 在组件调取使用的时候可以优化一些步骤

#### 我们使用react-redux需要做的事情

1. 当前根目录下使用Provider并且把store传过去，方便子孙将来使用；
2. 在每一个组件当中，我们不直接导出我们自己的组件，而是导出一个connect执行后的高阶组件
3. 以后所有的状态信息和派发方法都挂在到props里面了，而且之后我们不需要在写subscribe方法，他也帮我们做了

#### 提供了两个非常常用的组件：

**Provider：根组件**

- 当前整个项目都有Provider跟组件下，作用就是把创建爱你的store可以共内部任何后代组件使用
- 跟组件Provider里面只允许出现一个子元素
- 把创建的store基于属性传递给Provider（这样后代组件中都可以使用这个store了）

**connect：高阶组件**

#### 相对于传统的redux，我们做了步骤优化

1. 导出的不再是我们创建的组件，而是基于connect构造后的高阶组件

- export default connect([mapStateToPtops],[mapDispatchToProps])(Vote);
- mapStateToPtops：把redux容器中的状态信息遍历，赋值给当前组建的属性
- mapDispatchToProps：把redux中的dispatch派发行为遍历，也赋值给当前组件的属性

2. #### react-redux帮我们做的事情
>以前我们需要自己基于subscribe想事件池中添加方法，已达到容器状态信息改变，执行我们追加的方法，重新渲染组建的目的，但是现在react-redux帮我们做了这件事
>
> 所有用到redux容器状态信息中的组件，都会向事件池中追加一个方法，当容器状态改变，通知方法执行，把最新的状态信息当做属性传递给组件，组件的属性值改变了，组件也会重新渲染

3. #### react-redux帮我们做了一件事情；

4. 把action-creator中编写的方法（返回action对象的方法）自动构建成dispatch派发任务，也就是mapDispatchToProps;

```javascript
import {connect} from 'react-redux';

class Vote extends React.Component {
    constructor(props){
        super(props);
        this.state = {}
    }
    //.....
}

export default connect(state=>({...state.vote}),action.vote)(Vote);
```
