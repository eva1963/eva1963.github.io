---
title: ES6的语法学习
date: 2016-08-12 22:28:05
categories: JS
tags: js,es6
---
<h2>1. set Map</h2>
- [...new Set(ary)]去重方法
- 解决NaN不等于NAN ：Object.is(NaN,NaN);
- add 增加一项，重复的增加不上 
- delete 删除一项，返回值为true/false
- has查看有无当前项
- clear清空,返回值为undefined
- size长度
- set遍历的时候只有value值
  **下面这三个方法Array也有：**
- **set.keys();**
  -  **set.values();**
  -  **set.entries();**

``` javascript
    /* add 增加一项，重复的增加不上 */
    let set = new Set([1, 2, 23, 1, 12, 3, 54, 3]);
    set.add(1);
    /* delete 删除一项，返回值为true/false */
    set.delete(2);
    //    set遍历的时候只有value值
    set.forEach((item,index)=>{
      console.log(item,index);
    })
	 set.keys();
	set.values();
	set.entries();

     for (let key of set.keys()) {
	     console.log(key);
     }
```
<h2>2.  Symbol</h2>
- 基本数据类型 通过函数执行得到 不能使用new执行
- 唯一值
- 不能进行运算 因为不可以转数字 也不可以进行字符串拼接 这些都会报错
- 可以转为布尔值
- 当做属性名的时候只能用[""]的形式
- **Object.getOwnPropertySymbols()** 得到所有的属性命名是Symbol类型的key放到一个数组中
- **Object.keys()** 普通对象获取一个对象有多少键值对,但是获取不到Symbol类型的keys属性名
- Reflect.ownKeys方法可以返回所有类型的键名

<h2>3. async await</h2>
> async await都是ES6的语法糖
>
> 返回的Promise对象会以async function的返回值进行resolve，或者以该函数抛出的异常进行reject。
>
>    注意， **await 关键字仅仅在 async function中有效**。如果在 async function函数体外使用 await ，你只会得到一个语法错误（SyntaxError）
> ​    
> async/await的用途是简化使用 promises 异步调用的操作，并对一组 Promises执行某些操作。正如Promises类似于结构化回调，async/await类似于组合生成器和 promises。

- async 将一个函数变成promise对象
```
let fn = async function () {
   return 1;
};
fn().then(res=>{
   console.dir(res);
})
```
- await后面跟一个promise对象 如果不是默认将其变成一个resolve的promise 值作为resolve 的参数
```
  let fn = async function () {
        /*此时的promise是一个Promise对象，await就是一个语法糖，它把我们常用的new Promise().then()这种
         接收的形式直接简化为用await接收，如果下面接着要用，就直接变量promise.then()就好*/
        let promise = await new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve(1000);
            }, 1000)
        })
    };
    fn();
```
- await的结果就是resolve或者reject的参数
- await异步执行完成之后再去执行后面的代码
