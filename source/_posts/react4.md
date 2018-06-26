---
title: react基于JSX语法渲染的核心原理
date: 2018-2-24 22:11:35
categories: react
tags: react,JSX
---
#### JSX渲染机制：

1. 基于babel中的语法解析模块(babel-preset-react)把JSX语法编译为REACT.createElement(...)结构
2. 执行**React.createElement(type,props,children)**，创建一个对象，这个对象就叫虚拟dom，type:'div'标签名ref:keyprops:{id:'titleBox',className:'titleBox',style:...children:'coyote'//存放的是元素中的内容}__proto__:Object.prototype
3. **ReactDom.render(JSX愈发声称的对象，容器)**，基于RENDER方法把生成的对象动态创建为DOM元素，插入到指定的容器中；

不管是vue还是react框架，设计之初都是期望我们按照‘组件、模块管理‘的方式构建程序的，也就是吧一个程序花费为一个个组件来单独处理

【优势】

1. 有助于多人协作开发
2. 我们开发的组件可以被复用

#### react中创建组件有两种方式：

1. 函数声明式组件

2. 基于继承component类来创建组件

- 我们常常会单独建一个component文件夹，来专门放组件；

```javascript
/* 因为解析的时候要用到React.createElement所以，react是必须要引用的 */
import React from 'react';
/* 1. 函数式声明组件：函数的返回结果是一个新的JSX
 2. props变量存储的值是一个对象，包含了调取组建的时候传递的属性值
 *  */
export default function Dialog(props) {
    let title = props.lx === 0 ? '系统警告' : '系统提示';
    let {children} = props;

    /*children可能有也可能没有，可能是字符串也可能是一个数组，都代表组件里的祖孙元素*/
    return <section>
        <h2>{title}</h2>
        <div>{props.con}</div>
        {/*把属性中传递的子元素放到组件中的指定位置*/}
        {/*基于react中提供专门遍历children 的方法完成便利操作*/}
        {React.Children.map(children,item=>item)}
        {/*{children}*/}
    </section>
};
```

```javascript
import './self_JSX.js';
ReactDOM.render(<h1>
    {/* JSX 中调去组建只需要把组件当做一个标签调取即可 */}
    {/*如果属性值是数字就必要要大括号包住*/}
    <Dialog con="哈哈" lx={2} style={{color: 'red'}}/>
    <Dialog con="嘿嘿嘿" lx={0}>
        <span>123</span>
        <span>456</span>
    </Dialog>
</h1>,root)
```

**createElement在处理的时候遇见一个组件，返回的对象中，type就不在是字符串标签名了，而是一个函数（类），单数属性还是存在props里面的**

1. render渲染的时候我们需要做处理，首先判断type的类型，如果是字符串就创建一个元素标签，如果是函数/类，就把函数执行，把props的每一项（包含children）传递给函数


2. 在执行函数的时候把函数中return的JSX转化成新对象，通过createElement方法，把方法的结果也就是那个对象返回，紧接着按照以往的渲染方式创建DOM元素插入到指定容器当中即可

#### 基于继承Component类来创建组件

>  基于createElement把jsx转化为一个对象，当render渲染这个对象的时候，遇到type是一个函数或者类，不是直接创建元素，而是先把方法执行

>  如果就是函数式声明组件，就把它当做普通方法执行，方法中的this是undefined，把函数返回的jsx元素，其实也是解析后的对象进行渲染。

>  如果是类声明式的组件会把当前类new它执行，创建类的一个实例（当前本次调取的也就是他的实例），执行**constructor之后，会执行this.render()**，把render中返回的jsx拿过来渲染，所以**“类声明式组件，必须有一个render的方法，方法中需要返回一个jsx元素”**但是不管是哪一种方式，最后都会把解析出来的props属性对象作为实参传递给对应的函数或者类，而且每一个组件中都要导入react，需要React.createElement把jsx进行解析渲染

```react
/* 类继承 */
class Dialog extends React.Component {
     /* this.props是只读的，我们无法在方法中修改他的值，但是给其设置默认值或者设置一些规则，例如，设置是否必须传递，以及传递值的类型等 */
  //这个是react自带的，名字都是固定的defaultProps，不能改变，专门设置默认值的
    static defaultProps = {
        lx: '系统提示'
    };
//这个需要安装插件yarn add prop-types，然后用来检验某个字段的必传，类型什么的
    static propTypes = {
        /* 必传项且只能传递字符串或者只检验传入字符串，PropTypes.string
		设置的规则不会影响数据的渲染但是会在控制台抛出警告 */
        con: PropTypes.string.isRequired
    }
    
    constructor(props) {
        /* 继承了一些私有属性：this.props/this.refs/this.context */
      /* props:当render渲染并且把当前类执行创建实例的时候，会把之前JSX解析出来的props对象中的信息(可能有children)传递给参数props，‘调去组件传递的属性’ */
      /* 如果只写super()；虽然创建实例的时候把属性传递进来看了，但是并没有传递父组件，也就是没有把属性挂在到实力上，使用this.prop获取的结果是undefined */
     /* 如果super(props)在继承父类私有的时候，就把传递的属性挂载到了子类的实例上，constructor中就可以使用this.props了 */
        super(props);
        /* 相当于React.Component.call(this)*/
    }
  
// 即使在constructor中不设置形参props接收属性，执行supper的时候也不传这个属性，除了当前constructor里面不能直接使用this.props之外，其他的生命周期函数都可以使用this.props，（也就是执行完成constructor，react已经帮我们把传递的属性接收并且挂在到实例上）
    render() {
      //组件中的属性是调去组建的时候传递给组建的信息，而这部分信息是只读的(只能获取不能修改)，比如：this.props.con = '嘿嘿嘿'，就会报错
      console.lod(this.props);  //{name: "eva", className: "box", style: {…}, children: Array(2)}
        return <div>珠峰培训</div>
    }
}
//使用组件
ReactDOM.render(< div >
        <Dialog name="eva" className="box" style={{color: 'red'}} con='哈哈哈'>
            <span className="span"> i am a span1 </span>
            <span className="span2"> i am a span2 </span>
        </Dialog>
    </div>,,root);
```

### 总结：

创建组件有两种方式：

1. 函数式

- 操作非常简单
- 能实现的功能也很简单，只是简单的调取和返回JSX而已

2. 类式

- 操作相对复杂一些，但是可以实现更为复杂的功能
- 能够使用**生命周期函数**操作业务
- 函数式可以理解为静态组件（组件中的内容调取的时候就已经固定的，很难再修改），而类可以基于组件内部的状态来动态更新渲染的内容
- ...

```
/* propTypes是Facebook开发的插件，基于这个插件我们可以给组件传递的属性设置规则 */
import propTypes from 'prop-types';
```

REACT中的组件有两个非常重要的概念：

1. 组建的属性：调去组件的时候传递进来的信息
2. 组件的状态：自己在组件中设定和规划的（只有类声明式才有状态的管控，函数式组件声明不存在状态的管理）

#### 函数式：

>  **所谓的函数式组件，是静态组件**，和执行普通方法一样调去一次组件就把组件中内容获取到插入到页面当中，如果不重新调组件，显示的内容是不会发生任何改变的

真实项目中，只有调取组件，组件内容不会再次改变的情况下，我们才可能使用函数式组件；

```javascript 
/* 所以这个例子是很不好的！*/
function Clock() {
    return <section>
        <h3>当前北京时间是：{new Date().toLocaleString()}</h3>
    </section>
}
setInterval(()=>{
    /* 每隔一秒钟重新嗲奥曲一次组件渲染到页面中 */
    ReactDOM.render(<Clock></Clock>,root);
},1000)
```

#### 类式

>  组件状态类似于vue中的数据驱动，我们数据绑定的时候是基于状态值来绑定的，当修改组件内部状态的时候，对应的JSX元素也会跟着重新渲染（差异渲染：只把数据改变的部分重新渲染，基于DOM-DIFF算法完成的）

>  当代框架最重要的核心思想就是数据操控视图，也有视图影响数据，我们以后只需要改变数据，框架会帮我们重新渲染视图，告别手动操作DOM的时代，从而减少直接操作DOM，提高性能，有助于开发效率

```javascript

class Clock extends React.Component {
    constructor() {
        super();
        /* 初始化组建的状态(状态都是对象类型的):要求我们在constructor中把后期使用的状态信息全部初始化一下，约定俗成的语法规范 */
        this.state = {
            time: new Date().toLocaleString()
        };
    }

    componentDidMount() {
        /*react的生命周期函数之一，第一次组件渲染完成后触发（我们在这里只需要间隔1秒把state.time改变，这样react会自动帮我们把组件中的部分内容进行重新渲染）*/
        setInterval(() => {
            /* react中虽然下面方式可以修改状态，但是并不会通知react重新渲染页面，所以不要这么操作 */
            /*this.state.time = new Date().toLocaleString();*/

            /* 修改组件的状态：
             * 1. 修改部分状态，会用我们传递的对象和初始化的state进行匹配，只把我们传递的属性进行修改，没有传递的依然保留原始的状态信息
             * 2. 当状态修改完成，会通知react把组件JSX中的部分元素进行重新渲染
             */
            this.setState({
                time: new Date().toLocaleString()
            }, () => {
                /* 当通知react把需要重新渲染的JSX元素渲染完成后，执行的回调操作，类似于生命周期函数的componentDidUpdate,项目中一般使用钩子函数，而不是在这里使用回调 */
              /* 设置回调的原因：setState是异步操作 */
            })
        }, 1000)
    }

    render() {
        return <section>
            <h3>当前北京时间是：{this.state.time}</h3>
        </section>
    }
}

ReactDOM.render(<Clock></Clock>, root);

```
让类中的方法中的this变成当前实例，这样可以操作属性和状态等信息

```javascript
render() {
 return (
//    但是这种比较麻烦
   <button className='btn btn-success' onClick={this.support.bind(this)}>支持</button>
 )

support(ev){
 //    this是undefined，不是我们理解的当前操作的元素
 ev.target//通过事件源我们可以获取当前操作的元素,但是我们一般不会去操作Dom，我们保证数据驱动页面
 }
}
```

真实项目中，我们一般绑定的事件方法还是this.support ,但是这个方法我们用箭头函数，目的是为了保障函数中的this还是当前实例
```javascript
render() {
return (
  <button className='btn btn-success' onClick={this.support}>支持</button>
)
support = ev => {
  //    this是当前函数，不是我们理解的当前操作的元素
   this.setState({
n: ++this.state.n
      });
}
}
```

### REFS	

##### refs是react中专门提供通过操作Dom来实现需求的方式

> refs是一个对象，存储了当前组件当中所有设置ref属性的元素，且属性值是啥，refs对象中存储的元素的属性名就啥

### 受控组件

1. 基于数据驱动（修改状态数据，react帮助我们重新渲染视图）基于数据驱动完成的组件叫做“受控组件（受数据控制的组件）”
2. 基于ref操作DOM 实现视图更新的叫做'非受控组件'
3. 真实项目中，建议使用受控组件


