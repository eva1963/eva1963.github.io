---
title: React.Component与React.PureComponent的区别
date: 2018-08-05 10:37:05
categories: react
tags: component，pureComponent
---

​	最近换了工作，因为接手的项目中用到了Component和PureComponent，而自己之前一直使用React.Component而对PureComponent没有过多地了解到，所以就专门的查了一下，作为自己只是体系中的补充，也对自己接下来的项目开展做点铺垫，所以这里就谈谈两者的具体区别~

两者的本质区别：`PureComponent`已经定义好了`shouldUpdateComponent`而`Component`需要显示定义。

​	一般情况下，我们使用Component的时候，有时候我们虽然为了性能着想并不想无时无刻的去渲染页面，那么我们会设置一个`shouldComponentUpdate`周期函数，在这个周期函数中有我们自己手动来对比某些属性，当这些属性不同的时候我们才选择更新render执行，可能我说的也不是很清楚，网上扒了一个例子，参考如下：

```javascript
class CounterButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  shouldComponentUpdate(nextProps, nextState) {
    //在这里我们进行比较，这样可以节省性能
    if (this.props.color !== nextProps.color) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
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

从上面的例子我们可以看出，就是对比一些简单的值，而且有一种很多余的感觉，此时就是使用pureComponent的最好时机，React提供的`PureComponent`就会自动帮我们来进行`浅比较`，这样就不需要手动来写`shouldComponentUpdate`了；

你可能注意到了，上面我们提到了浅比较，虽然pureComponent自动为我们添加了`shouldComponentUpdate`周期函数，自己进行比较了，但是只是对props和state进行浅比较(shadow comparison)，当props或者state本身是嵌套对象或数组等时，浅比较并不能得到预期的结果，这会导致实际的props和state发生了变化，但组件却没有更新的问题，举个栗子：

```javascript
class ListOfWords extends React.PureComponent {
  render() {
    return <div>{this.props.words.join(',')}</div>;
  }
 }
 
class WordAdder extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      words: ['marklar']
    };
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    // 这个地方导致了bug
    const words = this.state.words;
    words.push('marklar');
    this.setState({words: words});
  }

  render() {
    return (
      <div>
        <button onClick={this.handleClick} />
        <ListOfWords words={this.state.words} />
      </div>
    );
  }
}
```

此外,React.PureComponent 的 shouldComponentUpate() 会忽略整个组件的子级。**请确保所有的子级组件也是”Pure”的。**



参考文章：https://segmentfault.com/a/1190000008402834