---
title: REACT-REDUX && REDUX
date: 2018-3-10 01:51:46
categories: react
tags: react,react-redux,redux
---

<p>直接上案例，说明react-redux对redux做了哪些优化处理：</p>

NO1：直接用redux写，比较麻烦：

```javascript
class VoteBase extends React.Component {
  constructor(props) {
     super(props);
      /!*
       * 真实项目中我们会把REDUX容器中的状态信息获取到，赋值给组件的状态或者是属性(REACT-REDUX)，这么做的目的是：当REDUX中的状态改变，我们可以修改组件内部的状态，从而达到重新渲染组件的目的
       *!/

      let reduxState = this.props.store.getState().vote;
      this.state = {
          ...reduxState //=>包含title/n/m所有管理的属性
      };
  }

  //=>在第一次加载执行，通过行为派发(VOTE_INIT)把REDUX中的信息赋值初始值
  componentWillMount() {
      this.props.store.dispatch(action.vote.init({
          title: '我长的帅不帅！',
          n: 0,
          m: 100
      }));
      let reduxState = this.props.store.getState().vote;
      this.setState({...reduxState});
  }

  //=>向发布订阅事件池中追加一个方法：监听REDUX容器中状态改变，状态改变重新渲染组件
  componentDidMount() {
      this.props.store.subscribe(() => {
          let reduxState = this.props.store.getState().vote;
          this.setState({...reduxState});
      });
  }
}
```

NO2：react-redux改版后：

```javascript
class VoteBase extends React.Component {
    constructor(props) {
        super(props);
        console.log(this.props);
    }

    componentWillMount() {
        this.props.init({
            title: '我长的帅不帅?',
            n: 0,
            m: 100
        });
    }

    render() {
        let {title, n, m} = this.props;

        return <div className='panel panel-default'>
            <div className='panel-heading'>
                <h3 className='panel-title'>
                    {title}
                </h3>
            </div>
            <div className='panel-body'>
                支持人数：<span>{n}</span>
                <br/><br/>
                反对人数：<span>{m}</span>
            </div>
        </div>;
    }
}

/*//=>把REDUX容器中的状态信息遍历，赋值给当前组件的属性（state）
let mapStateToProps = state => {
    //=>state:就是REDUX容器中的状态信息
    //=>我们返回的是啥，就把它挂载到当前组件的属性上（REDUX存储很多信息，我们想用啥就返回啥即可）
    return {
        ...state.vote
    };
};*/

/*
//=>把REDUX中的DISPATCH派发行为遍历，也赋值给组件的属性（ActionCreator）
let mapDispatchToProps = dispatch => {
    //=>dispatch:STORE中存储的DISPATCH方法
    //=>返回的是啥，就相当于把啥挂载到组件的属性上（一般我们挂载一些方法，这些方法中完成了DISPATCH派发任务操作）
    return {
        init(initData) {
            dispatch(action.vote.init(initData));
        }
    };
};
export default connect(mapStateToProps, mapDispatchToProps)(VoteBase);
*/

export default connect(state => ({...state.vote}), action.vote)(VoteBase);//=>REACT-REDUX帮我们做了一件事情，把ACTION-CREATOR中编写的方法（返回ACTION对象的方法），自动构建成DISPATCH派发任务的方法，也就是mapDispatchToProps这种格式
```

