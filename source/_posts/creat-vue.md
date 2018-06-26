---
title: vue脚手架搭建项目整体流程
date: 2018-03-11 21:49:35
categories: VUE
tags: vue,js,node,webpack
---
1. 执行`vue init webpack shoppingmall`构建一个完整的vue脚手架项目；
   安装axios -S
   安装vant -S

在main.js中引入vant插件，可以使用官方文档推荐的装入`npm i babel-plugin-import -D`按需引入你所需要的插件；

先写一个公用组件，然后其他的模块组件也都写好，在去router里面去配置路由；

配置路由首先要把路由所用的组件都引进去，VueRouter也要作为插件引进去，就是vue.use(router);

组件通用注册：

```javascript
import { Icon, Tabbar, TabbarItem, Lazyload } from 'vant'

// Vue.use(Vant);
Vue.use(Icon).use(Tabbar).use(TabbarItem).use(Lazyload);
```

组件用的次数比较少：直接在在vue里面写：

```javascript
import { getSlidesData } from "../api";
import {Swipe, SwipeItem} from 'vant';

components: {
    /* 如果轮播图这个插件只用了这一次，我们可以直接在这个文件里面注册，注册方法如下： */
    /* 如果插件用的比较频繁，就直接main里面用vue.use的方式注册 */
  [Swipe.name] : Swipe,
  [SwipeItem.name] : SwipeItem
},
  async created() {
    let slideData = await getSlidesData();
    this.slides = slideData.slides;
  }
```

自定义的iconfont字体，放到asset文件里面，然后在App.vue里面引入iconfont.css文件，使用的时候直接用字体文件的class名字即可

- ** Vue.use(Router)**是在router的index.js里面作为插件使用的；


- **main.js**里面是把**vue相关的使用的插件用use写进去**，两者不同；

你在router里面路由参数配置了之后，在页面中想使用就是方法就是$router.pash(),使用属性就是$route.params.XXX;

```javascript
//两种路由导航方式：
<div @click="$router.push('/order/daifukuan')">
    <van-col span="6">
      <p><van-icon name="daifukuan"></van-icon></p>
      <p>代付款</p>
    </van-col>
  </div>
  <div @click="$router.push({name: 'order',params: {type:'daifahuo'}})">
    <van-col span="6">
      <p><van-icon name="daifahuo" info="9"></van-icon></p>
      <p>代发货</p>
    </van-col>
  </div>
```




