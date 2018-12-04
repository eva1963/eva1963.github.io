---
title: react更新相关函数
date: 2018-10-29 18:06:59
categories: react
tags: react
---

### react相关

>  最近react刚刚公布16.3版本的公布，之前一直没时间来仔细整理一下，就听大家说will系列的钩子函数都要被废弃了，这会有时间就来整理一下到底更新了一些什么东西吧~

### 新增的钩子函数：

我们都知道，在16.0刚推出的时候，是增加了一个`componentDidCatch`生命周期函数，这只是一个增量式修改，完全不影响原有生命周期函数；但是，到了React v16.3，大改动来了，引入了两个新的生命周期函数：

- getDerivedStateFromProps
- getSnapshotBeforeUpdate

1. `getDerivedStateFromProps`实际上就是用来取代以前的函数`componentWillReceiveProps`，但他们又有一些不同，准确来说，componentWillReceiveProps是在update的时候被调用的，只有在父组件传递给子组件的props发生了变化的时候才会调用，而getDerivedStateFromProps是在Updating和Mounting的时候都会被调用；

2. `getSnapshotBeforeUpdate`函数会在render之后执行，而执行之时DOM元素还没有被更新，给了一个机会去获取DOM信息，计算得到一个snapshot，这个snapshot会作为componentDidUpdate的第三个参数传入例如：

   ```react
   getSnapshotBeforeUpdate(prevProps, prevState) {
       console.log('#enter getSnapshotBeforeUpdate');
       return 'foo';
     }
   
     componentDidUpdate(prevProps, prevState, snapshot) {
       console.log('#enter componentDidUpdate snapshot = ', snapshot);
     }
   ```

### 即将被废弃的钩子函数：

另外之前不建议使用的几个钩子函数都被标识为不安全，依然不建议使用，或许在后期或被直接废弃掉：

- UNSAFE_componentWillReceiveProps
- UNSAFE_componentWillMount
- UNSAFE_componentWillUpdate

### 更新前后对比图：

- 修改之前的周期函数概览图

![@修改之前的周期函数概览图](https://pic4.zhimg.com/80/v2-8c9f2b2eebc3449da805e8bd0deced47_hd.jpg)

- 修改之后

  ![@修改之后](https://pic1.zhimg.com/v2-930c5299db442e73dbb1d2f9c92310d4_r.jpg)

当然， 唯一的值得吐槽的是，改了之后函数名一如以往的长~