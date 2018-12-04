---
title: REACT-ROUTER
date: 2018-03-04 01:02:19
categories: react
tags: react,react-router
---
### REACT-ROUTER

#### SPA：单页面应用

> 只有一个页面，所有需要展示的内容都要在这一个页面中实现切换，webpack只需要配置一个入口即可（移动页面应用居多，或者PC端管理系统类也是单页面应用为主）

<h3>如何使用单页面应用？（SPA发展史）</h3>

1. 如果项目是基于服务器渲染的，后台语言当中可以基于‘include’等技术，把很多部分拼凑在一起，实现组件化，插件化开发，最后也可以实现单页面应用
2. 基于IFRAME实现单页面应用
3. 模块化开发

- AMD：require.js
- CMD:SEA.JS

基于这些思想吧每一部分单独写成一个模块，最后基于grunt/gulp/fis等自动化工具，最后把搜偶有的模块都合并到首页面中（包括HTML,CSS,JS），通过控制那些页面显示隐藏来实现单页面应用开发

弊端：由于首页中的内容包含了所有模块的信息，所以首次加载非常慢；

4. 基于VUE/REACT实现模块化组件化开发，基于他们提供的路由实现SPA单页面开发，基于webpack打包等



#### MPA：多页面应用

> 一个项目由很多页面组成，整个项目是页面之间跳转完成的；PC端多页面应用居多
>
> 在基于框架开发的时候需要在webpack中配置多入口，每一个入口对应一个页面；

#### 安装react-router

> 3.00+及以前版本，成为react-router，4.00+成为react-router-dom

```javascript
npm i react-router-dom -S
```

#### BrowserRouter VS HashRouter

> 两种常用的路由实现思想，BrowserRouter是浏览器路由，HashRouter是哈希路由；

【BrowserRouter】

> 它是基于H5中的history API来保持UI和URL同步的，真实项目中应用不多，一般只有当前项目是基于服务器端渲染的，我们才会使用浏览器路由；服务器端得到数据然后进行拼接然后客户端只需要拿到渲染即可
>
> 一般这么做，大部分都是为了做SEO搜索引擎优化；

【HashRouter】

> 真实项目中(前后端分离的项目)，客户端渲染，我们经常使用的是HASH路由来完成的；它依据相同的页面地址，不同的哈希值，来规划当前的页面哪一个组件呈现渲染，它基于原生JS构造了一套类似与history API的机制，每一次路由切换都是基于history stack完成的

1. 当前项目一旦使用HashRouter，则在页面地址后面加上‘#/'，也就是hash默认值是一个’/'，我们一般让其显示首页内容
2. HashRouter中只能出现一个子元素；
3. HashRouter机制中我们需要根据hash不同展示不同的组件内容，此时我们需要使用router；

#### ROUTE,两个参数PATH&&COMPONENT

> 校验规则：默认情况下会和每个route都做校验，哪怕之前已经有了校验成功的了，他也还是会和每一个对一遍,这个问题我们用Switch来解决即可

**path:** 设置匹配地址，但是不是默认不是严格匹配

**component:**一旦hash值和当前route的path相同了，则渲染component指定的组件

**exact**：让path的匹配相对严谨严格，只有url的hash值跟path的相等才可以匹配到；

**render**：里面参数是一个回调函数，用作处理权限校验较多；当页面的hash地址跟path匹配，会把render执行；渲染组件之前验证是否存在权限，不存在做一些特殊处理；

**Switch**：只要有一种情况校验成功，就不再向后校验了

#### Redirect

参数：

**TO**【字符串】：重定向到一个新的地址；

**TO【Object】**：重定向到一个新地址，制定了更多的信息而已,里面包含的：

- **pathname**:定向的地址；
- search：给定的地址问号传参；（真实项目中，我们有些时候会根据是否存在问号参数值来统计是正常进入首页还是非正常跳转过来的，也可能根据问号值做不一样的事情）
- state：给定向后的组件传递一些信息

**PUSH**：如果设置了这个属性，当前跳转的地址会加入到history stack中一条记录

**FROM**：设置当前页面来源的页面地址，举个栗子：
```javascript
  //如果当前请求的地址是/custom，我们就把它重定向到/custom/list
  <Redirect from="/custom" to="/custom/list"/>
```

#### Link

**TO**用法和上面的差不多；

**replace** 是替换HISTORY stack中当前的地址，还要追加一个新的地址

原理：基于LINK组件渲染，渲染后的组件就是一个A标签

#### NavLink

> 跟link类似，都是用来实现路跳转的，不同在于NavLink组件在当前页面hash地址和组件对应地址相吻合的时候，会默认给组件加一个active样式

其他的跟LINK用法都一样，加了一个**activeClassName,activeStyle**

#### withRouter

> 把一个非路由管控的组件，模拟成为路由管控的组件

```javascript
<Route path='/' component={Nav} }
//变为受路由管控的
widthRouter(connect()(Nav));
```

#### 受路由管控组件的特点：

1. 只有当前页面的哈希地址和路由指定的地址匹配，才会把对应的组件渲染（withRouter是没有地址匹配，都被模拟成为受路由管控的）

2. **路由切换的原理：**凡是匹配的路由都会把对应的组件内容重新添加到页面中，相反，不匹配的都会在页面中移除掉，下一次重新匹配上，组件需要重新渲染;每一次路由切换的时候的哈西路由地址改变，都会重新从一级路由开始重新校验一遍

3. 所有受路由管控的组件，在组建的属性Props上都默认添加了三个属性：<h3>（重要！！！）</h3>

   - history

     > 相当于一个路由池子，里面存放的是你所有push进来的路由

     - push：向路由池子中添加一条新的信息，达到切换指定路由地址的目的；this.props.history.push('/plan')，JS中实现路由切换
     - go：跳转到指定的地址
     - goBack：相当于go(-1)
     - goForward：相当于go(1)

   - location

     > 获取当前哈西路由渲染组建的一些信息

     - pathname：当前哈西路由的地址
     - search：当前页面的问号传参
     - state：基于redirect。link/NavLink中的to，传递的是一个对象，对象中编写的state，就可以在location.state中获取到

   - match

     > 获取的是当前路由匹配的结果

     - params：如果当前匹配的是地址路径参数，则这里可以获取传递参数的值

#### 整体路由如何运用，项目实际应用如下：

```javascript
render(
    <Provider store={store}>
        <HashRouter>
            {/* 基于hash路由来展示不同页面 */}
            <div>
                <Nav/>
                <Switch>
                    {/* 头部公共部分 */}
                    <Route path="/" exact component={Home}/>
                    <Route path="/custom" component={Custom}/>
                    <Route path="/plan" component={Plan}/>
                    <Redirect to="/?lx=unsafe"/>
                </Switch>
            </div>
        </HashRouter>
    </Provider>,
    document.getElementById('root')
);
```

在SPA路由管控的项目中，从列表跳转到详情，总需要传递一些信息给详情，依此展示不同的信息，传递给详情页信息的方式我们有很多种：

【不推荐】

redux

本地存储

【推荐】

- 问号传参

- state传值(弊端：一旦页面刷新，上一次传递过来的state值就丢了啊)

- URL地址传参：把参数当做地址的一部分

  > path = '/custorm/detail/:id'



#### redux中间件

> 在真正处理之前，做一些事情

**redux-logger**：能够在控制台清晰地展示出，redux操作的流程和信息（原有状态，派发信息，修改后的状态）

**redux-thunk**：处理异步的dispatch派发

**redux-promise**：在dispatch派发的时候，可以使用Promise

