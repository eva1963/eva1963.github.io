---
title: 数组遍历的几个方法
date: 2016-09-15 22:25:19
categories: JS
tags: js,es6
---

<H1 style="text-align=center">数组遍历方法<H1>
>  除了reduce，reduceRight两个，下面的方法的**第二个参数都可以用来改变this的**

- **forEach**
```
//对每一项做处理，但是原数组不会改变，而且没有返回值
    let ary1 = [1, 2, 3, 4, 5, 0];
    let ary2 = ary1.forEach(function(a){
        console.log(this);   //['this','is','change']
        a *= 2;
        console.log(a);    //1,4,6,8,10,0
    },['this','is','change']);
  console.log(ary1, ary2);// [1, 2, 3, 4, 5, 0]和undefined
```
- **map**
```
 // 原数组不会改变
    // 返回值：是回调函数里面return的每一项值
    let ary1 = [1, 2, 3, 4, 5, 0];
    let ary2 = ary1.map(function(a){
        console.log(this);  //this也变了
        return a *= 2;
    },['this','is','change']);
    console.log(ary1, ary2);    //(6) [1, 2, 3, 4, 5, 0] (6) [2, 4, 6, 8, 10, 0]
```
- **find**  
```
	返回值为3,而且找到一项就停止了 ，返回的只有第一次找到的那个值
    let ary2 = [1, 2, 3, 4，5，0].find(a =>{return a > 2});
```
- **findIndex**
```
/*查找某一项的索引*/
	返回值为索引
    let ary2 = [1, 2, 3, 0].findIndex(a =>{return a > 2});//=>2
```
- **filter**
```
/*查找某一项的索引*/
    let ary2 = [1, 2, 3,4，5， 0].filter(a =>{return a > 2});
    //返回值为数组[3, 4, 5]
```
- **some**
```
    // 原数组不会改变
    // 返回值：是布尔值
    let ary1 = [1, 2, 3, 4, 5, 0];
    let ary2 = ary1.some(function(a){
        console.log(this);  //this也变了
        return a >= 2;
    },['this','is','change']);
//    只遍历了两次，第三次的时候发现符合条件，终止遍历
    console.log(ary1, ary2);    //(6) [1, 2, 3, 4, 5, 0] true
```
- **every**
```javascript
//代码如上参考some
跟some的区别在于：
以上代码，只遍历了1次，第2次的时候发现不符合条件，就会终止遍历，返回false
```
- **reduce**
```
    // 原数组不会改变
    // 返回值：求和结果
    let ary1 = [1, 2, 3,4,5];
    let ary2 = ary1.reduce(function(prev,cur){
        console.log(prev, cur);
        return prev+cur;//1+2+3+4+5 = 15
    });
    console.log(ary1, ary2);    //[1, 2, 3, 4, 5] 15
```
- **reduceRight**
> 它跟reduce的唯一区别是它是倒着处理的： 5+4+3+2+1 = 15
