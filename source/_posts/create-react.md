---
title: react脚手架搭建项目整体流程
date: 2018-04-12 21:45:47
categories: react
tags: react,js,node
---
1. 首先保证在node环境下，全局已经安装了react脚手架，命令如下：

```javascript
npm i create-react-app -g
```

<em>tips: 查看本地node里面安装了那些全局插件`npm root -g`</em>

<h3>如果想要用less编写做以下处理：</h3>

2. 执行`yarn eject`使得项目中的配置文件从node_modules里面暴露出来的，方便我们更改；

- 执行完第二步才做之后，项目文件目录中会多出两个文件夹'config'&&'script'；

  #### 修改操作：

  >  修改webpack.config.dev.js&&webpack.config.prev.js里面对应的css处理部分，按如下修改：

  ```javascript
  {
    test: /\.(css|less)$/,
      use: [
        require.resolve('style-loader'),
        //....省略中间不变部分
        {
          loader: require.resolve('less-loader')
        },
      ],
  },
  ```

3. 如果想修改默认打开的端口可以在script文件夹中的start.js中修改如下内容：

```javascript
// 下面的3000指的就是端口
const DEFAULT_PORT = parseInt(process.env.PORT, 10) || 3000;
```

+ 或者使用命令直接改变node全局变量中的PORT（因为react中做了process.env.PORT这个处理）:
  + MAC环境下：执行`PORT=63342 yarn start`即可
  + WINDOW环境下：执行`set PORT=63342&&yarn start`即可

tips: http设置为https更上面差不多,`HTTPS=TRUE yarn start`

4. 想要使用第三方插件如booststrap这些样式的时候，直接在入口文件index.js里面引入即可；
5. 如果使用到redux/react-redux的时候，首先在生产环境中安装这两个插件；

+ 在src文件中建一个store文件夹，用来存放相关的reducer&&action&&action-types；
  + **store文件下有一个index.js：它创建了一个最大的中央管理区；**里面可能存放着各种小管理区，然后通过combineReducer方法把这个小管理区的东西全都汇总到中央管理区；然后ES6导出这个中央管理区store给开发用，谁想用就import就成；
  + **reducer文件夹：每一个模块的reducer都在这里，最后由index.js文件进行合并；**文件夹下也有一个自己的index.js,除此之外可能还有vote.js/person.js等文件，这些文件都创建了一个自己的管理区块reducer，然后导出，最后在reducer文件夹下的index.js文件里整个汇总成一个中央管理区，为了区分。每一个模块都有自己的命令空间，防止相互之间冲突；所以使用的时候我们经常会这样写：this.props.store.getState().vote，这就是拿到vote命名空间下的所有变量，也就是vote.js文件中的变量，这样使我们的管理变得十分简便；
  + **action文件夹：每个模块修改redux容器中状态，派发的任务都在这；**存放的也有index.js/vote.js/person.js等文件，每一个模块里导出自己的方法，方法中return一个对象，这个对象就是以后点击事件中dispatch派发事件的参数，通过这个参数type: VOTE_SUPPROT，派发事件根据这个表示触发管理区reducer的第二个参数action执行对应的操作。这个操作紧接着就会触发事件池subscribe自上而下执行，subscribe在执行之前我们必须手动拉取一下最新值：this.props.store.getState()，接着去执行下面的代码，因为state发生改变，也就触发了render重新渲染整个页面，这样一个完整的点击事件就完成了
  + **action-types.js：统一管理所有派发的标识**，这个文件是一个中间层，他把所有的派发标识都整理在一起，我们使用的时候直接引入使用即可；
