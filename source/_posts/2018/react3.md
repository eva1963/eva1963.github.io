---
title: react之复合组件
date: 2018-3-1 20:10:09
categories: react
tags: react,复合组建
---
#### 父组件传值给子组件：

**属性传递：老子传小子，小子不能传给老子**：基于属性传递即可（而且是单方向的：只能父亲通过属性把信息传给儿子，儿子不能直接把信息作为属性传递给父亲）

后期子组件中的信息需要修改，，可以让父组件传递给子组件的信息发生变化（也就是子组件接受的属性发生变化）子组件会重新渲染，触发componentWillReceiveProps钩子函数

只要实现点击子组件Body按钮的时候可以修改父组件Panel的状态信息N,Panel状态改变，则会重新执行render渲染，而重新渲染的时候子组件Head也会接收到最新渲染的N的属性值，从而是HEAD组件也重新渲染，从而达到改变子组件里面值的目的；

#### 子组件修改父组件：

1. 把父组件中的一个方法作为属性传递给子组件
2. 在子组件当中，把基于属性传递进来的方法，在合适的时候执行，相当于在执行父组件中的方法：而这个方法中完全可以操作父组件中的信息

```react

class Head extends React.Component {
    render() {
        return (
            <div className="panel-heading">
                <h3 className="panel-title">点击次数:{this.props.count}</h3>
            </div>
        )
    }
}


class Body extends React.Component {
    render() {
        return (
            <div className="panel-body">
                <button className="btn btn-success" onClick={this.props.callback}>点我啊
                </button>
            </div>
        )
    }
}

//父组件Panel
class Panel extends React.Component {
    constructor() {
        super();
        this.state = {
            n: 0
        }
    }
    // 这个是要传给子组件的方法
    fn = () => {
        this.setState({
            n: ++this.state.n
        })
    }

    render() {
        return (
            <div className="panel panel-danger">
                <Head count={this.state.n}/>
                {/*通过属性吧方法传递给子组件*/}
                <Body callback={this.fn}/>
            </div>
        )
    }
}

ReactDOM.render(<Panel/>, root);
```



在JSX中要使用图片等资源的时候，资源的地址不能使用相对值，因为经过webpack编译后，资源地址的路径已经改变了，原有的相对地址无法找到对应资源，此时我们需要给予ES6module或者commonJS等模块导入规范，把资源当做模块，导入进来；

或者我们使用的图片地址都是网络地址；

#### 数据挂载的几种情况：

1. 放属性props上，我会拿来用，但是不会改变；
2. 放状态this.state上，改变影响视图渲染；
3. 一类是直接挂在到当前实例上this，方便我下次拿来用户
