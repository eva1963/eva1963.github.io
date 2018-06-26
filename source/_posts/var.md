---
title: VAR && LET
date: 2017-04-01 19:21:52
categories: JS
tags: js，var，let
---

####只对等号左边的进行变量提升
```
sum();	//2
fn();//报错   只对等号左边的进行变量提升
var fn = function(){
console.log(1);
}	//	只对fn声明，不定义
function sum（）{console.log(2)}；//声明和定义都完成了
```

在当前作用域下，不管条件是否成立都要进行变量提升,
- 带var的还是只声明
- 带function的在老版本浏览器渲染机制下，声明和定义都处理，为了迎合ES6中的块级作用域，新版本浏览器对于函数（在条件判断中的函数，不管条件是否成立，都只是先声明未定义，类似于var）
```
console.log(a);	//undefined   条件判断下a已经声明了
if (1 === 2) {
    var a = 12;
}
console.log(a);	//undefined   条件不成立，不赋值
```
```javascript
function fn() {
    console.log(b); //报错
    b = 13;//相当于给window.b设置值
    console.log('b' in window);//true
    console.log(b);//13
}
fn();
console.log(b); //13
```
```javascript
var n = m =[12,23];
n === m; //true
/*
 * var n;
 * 准备值（AAAFFF222）
 * n = AAAFFF222
 * m = AAAFFF222
*/
```
```javascript
sum();	//2
fn();//报错   只对等号左边的进行变量提升
var fn = function(){
console.log(1);
}	//	只对fn声明，不定义
var fn = function sum(){
console.log(1);
}//给匿名函数设置名字其实没有实际的用于，在函数外面是无法获取这个函数名的，只有在匿名函数里面才能调用，之后递归的时候可能会用到；
function sum（）{console.log(2)}；//声明和定义都完成了
setTimeout（）{
function (){
}
}
```
#### 条件判断下的变量提升
在当前作用域下，不管条件是否成立都要进行变量提升,
- 带var的还是只声明
- 带function的，新版本浏览器对于函数（在条件判断中的函数，不管条件是否成立，都只是先声明未定义，类似于var），在老版本浏览器渲染机制下，声明和定义都处理，为了迎合ES6中的块级作用域
```javascript
console.log(a);	//undefined   条件判断下a已经声明了
if (1 === 2) {
    var a = 12;
}
console.log(a);	//undefined   条件不成立，不赋值
```

```javascript
console.log(fn);   //undefined
if(1==1) {
    console.log(fn);    //函数本身
// 当条件成立，进入到判断体当中，在ES6中他是一个块级作用域，之后的第一件事是类似于变量提升一样，先把FN声明和定义了，也就是在代码执行之前，FN就已经了
    function fn(){
        console.log(2);
    }
}
console.log(fn);  //函数本身
```

```
// 360浏览器面试题=====考点：条件判断下的变量提升
f = function () {return true;};// f和g相当于给window添加了属性
g = function () {return false;};// f和g相当于给window添加了属性
~function () {
    // 新版本浏览器中 ===》形成一个私有作用域，进行变量提升，新版本浏览器中只对函数进行声明不定义，所以只是声明了一个g，那么此时g是undefined；
    // 老版本浏览器中 ===》形成一个私有作用域，进行变量提升，老版本浏览器只对函数进行声明和定义，g此时既声明也定义了，那么此时g是代码字符串；
    if (g() && [] == ![]) {
        f = function () {return false;};
        function g() {
            return true;
        }
    }
}();
console.log(f());
console.log(g());
```


#### 重命名问题的处理
1. 带var和function关键字声明相同的名字，这种也算是重名了（其实是一个fn，只是存储的类型不一样）
2. 关于重名的处理：如果名字重复了不会重新的声明但是会重新的定义，就是赋值（不管是变量提升还是代码执行解读那皆是如此）
```javascript
/** 变量提升：
 *   fn = ...（1）
 *      = ...（2）
 *      = ...（3）
 *      = ...（4）
 */
fn();//=>4
function fn() {console.log(1);}
fn();//=>4
function fn() {console.log(2);}
fn();//=>4
var fn = 100;//=>带VAR的在提升阶段只把声明处理了,赋值操作没有处理,所以在代码执行的时候需要完成赋值 FN=100
fn();//=>100() Uncaught TypeError: fn is not a function
function fn() {console.log(3);}
fn();
function fn() {console.log(4);}
fn();
```

####let创建的变量不存在变量提升
> **在ES6中，基于let以及const等方式创建变量或者函数，不存在变量提升机制；而且切断了全局变量和window属性的映射机制**
>
> **在相同的作用域当中，基于let不能声明相同的名字的变量(不管在当前作用域下声明了变量，)**
	- 解释：虽然没有变量提升机制但是在当前作用于代码自上而下执行之前，浏览器会做一个重复性检测（语法检测）：自上而下查找当前作用域下所有变量一旦发现有重复的直接抛出异常，代码也不再执行了，虽然没有把变量提前声明定义但是浏览器已经记住了当前作用域下有哪些变量
```
var b =12;
let b = 16;   
console.log(b);		//报错
//--------------------
a = 12;
console.log(a);		//报错
let a =10;
console.log(a);			//上面报错，这个不执行了
```

* 基于let和基于var变量在基于私有变量和作用域链机制上是一样的
* 如果创建变量是基于let创建的，就按ES6的机制渲染，否则就按老版本渲染机制

#### JS暂时性死区
```javascript
ES5:
// 在原有浏览器渲染机制下是基于typeof逻辑运算符检测一个未被声明过得变量不会报错，直接console出来的a是报错，但是typeof  a的结果是undefined，并不会报错，这可以理解为浏览器的一个bug，是浏览的一个暂时性死区
console.log(a);
console.log(typeof a);

ES6:
// 如果当前变量是基于ES6的语法处理的，在没有声明这个变量的时候使用typeof a;就会报错，不会使undefined，解决了原有的暂时性死区
console.log(a);
console.log(typeof a);
let a;
```


