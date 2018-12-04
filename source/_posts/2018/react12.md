---
title: react问题汇总
date: 2018-11-20 16:58:08
categories: react
tags: react
---

#### 为什么不在componentWillMount里写AJAX获取数据的功能?

componentWillMount在render之前执行，早一点执行早得到结果。要知道，在componentWillMount里发起AJAX，不管多快得到结果也赶不上首次render，而且componentWillMount在服务器端渲染也会被调用到（当然，也许这是预期的结果），这样的IO操作放在componentDidMount里更合适。在Fiber启用async render之后，更没有理由在componentWillMount里做AJAX，因为componentWillMount可能会被调用多次，谁也不会希望无谓地多次调用AJAX吧

#### 为何要用setState去更新数据，而不是直接变更数据?

- 直接变更数据也是可以的，但是它不会触发页面重新渲染
- react自带的setState里面不仅仅是对数据做了变更，它还利用diff算法去比较每次的变更，而且会触发render方法执行，从而使得页面重新渲染

#### setState是一个异步操作！

#### setState方法的第二个参数：

setState变更数据，只有执行了render之后，这个数据才真正的被更改掉，在此之前我们想要拿到最新的数据，就可以用到setState的第二个参数，通过在回调函数里面去拿到最新的值；