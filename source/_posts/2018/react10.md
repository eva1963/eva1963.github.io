---
title: react创建组建的四种方式
date: 2018-08-04 11:10:28
categories: react
tags: component,pureComponent,createClass,Function
---
react创建组件的方式有很多种，但是一般情况下公司里可能只用一种react.Component,但是其他三种方式我们还是应该了解一些，因为有时候公司不用是因为大家不会用，然而存在即合理，既然有这么多种方式肯定就有它本身的优点，或者对于性能有很大的提升，或者对于精简代码有帮助，所以本宝认为有些东西还是需要自己去深入理解学习才能在最大程度的去使用它；以下就是创建组建的四种方式，除了第一种不常用之外我认为其他三种都还是运用比较多的，所以建议如果宝宝你看我的不是很理解的话可以根据我查阅的资料链接或者自己再去挖掘以下~

<h2 style="color:'red'">**createClass**</h2>

> 不常用且基本不用，官方都建议放弃了的，我们提一嘴

```javascript
var React = require("react");
var FirstComponent = React.createClass({
  propTypes: {
    name: React.PropTypes.string 
    //属性校验 (string, number, bool, func, array, object...... )
  },
  
  getDefaultProps: function() {
    return {
      name: 'Mary' 
    };
    // 初始化props
  }, 
    
  getInitialState: function() {
    return {count: this.props.initialCount}; 
    //初始化props
  },
  
  handleClick: function() {
    //.....
  },
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});
module.exports = FirstComponent;
```

**<h2 style="color:'red'">Component**</h2>（最常用，里面包含构造函数，周期函数，状态值等）

```javascript
import React from 'react';
class FirstComponent extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
    this.state = {
       count: props.initialCount
    };
  }
  static propTypes ＝ {
    name: React.PropTypes.string
  }
  statice props = {
    name: 'Mary'
  }
  handleClick() {
    //点击事件的处理函数
  }
  
  render() {
    return <h1 onClick={this.handleClick}>Hello, {this.props.name}</h1>;
  }
}
export default FirstComponent;
```

<h2 style="color:'red'">**pureComponent**</h2>

>  大多数情况下， 我们使用`PureComponent`能够简化我们的代码，并且提高性能，但是PureComponent的自动为我们添加的`shouldComponentUpate`函数，只是对`props`和`state`进行浅比较`(shadow comparison)`，当`props`或者`state`本身是嵌套对象或数组等时，浅比较并不能得到预期的结果，这会导致实际的`props`和`state`发生了变化，但组件却没有更新的问题。当然还是有解决的方法的,比如使用`Immutable.js`来创建不可变对象，通过它来简化对象比较，提高性能

```javascript
class CounterButton extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}
```

<h2 style="color:'red'">**Function Component**</h2>

```javascript
import React from 'react';

Button.propTypes = {
  day: PropTypes.string.isRequired,
  increment: PropTypes.func.isRequired,
}

const Button = ({
  day,
  increment
}) => {
  return (
    <div>
      <button onClick={increment}>Today is {day}</button>
    </div>
  )
}
```
