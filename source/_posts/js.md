---
title: JS之什么是变量
date: 2017-06-22 21:14:11
categories: JS
tags: 前端
---

<h2>什么是变量（variable）？</h2>

所谓变量，他不是一个具体的值，只是一个用来存储具体值的容器或者代名词；

基于ES语法规范，在js中创建变量有以下方式：
ES3中的：
    - var
    - function（函数也是变量只不过存贮的值是函数类型的而已）
es6中的：
    - let 创建变量
    - const 创建常量
    - import 基于ES6的模块规范导出需要的信息
    - class  基于ES6创建类

```javascript
var a= 1;
/*
* 语法： var  [变量名]= 值；
*      let  [变量名] = 值；
*      const  [变量名] = 值；
*      function 变量名(){}
*/
```

创建变量命名的时候要遵循一些规范：
- 严格区分大小写
```
例如：
 var n=12;
 var N=12;  //两个N不是同一个变量
```

- 遵循驼峰命名法：按照数字字母下划线$来命名，数字不能开头；下划线放前面的一般都是公共变量
```
例如：
var  studentInfo/student_info/
_studentInfo(下划线放前面的一般都是公共变量)
$studentInfo(一般存储的是JQ元素)
```

- 不能使用关键字或保留字：在js中有特殊含义的叫关键字，未来可能变成关键字的叫保留字；

----

<h2>8种数据类型</h2>
- 基本数据类型（值类型）
    + 数字 Number
    + 字符串 string
    + 布尔 boolean
    + null
    + undefined

```javascript
[基本数据类型]
  number -> 数字类型中有一个特殊的值  NAN   代表不是数字  但是它是数字类型的；
  string -> js中所有单引号或者双引号包裹起来的都是字符串，里面的内容是当前字符串中的字符，由0到多个组成；
  boolean -> true/false
- 引用数据类型
+ 对象 object
   + 普通对象
   + 数组对象
   + 正则对象
   + 日期对象
+ 函数
[引用数据类型]
  普通的对象 -> 由大括号包含起来，里面包含多组键值对（属性名：属性值）；
  数组对象 -> 由中括号包含起来，包含零到多项内容
  正则对象 -> 由元字符组成的一个完整的正则
       var reg = /-?(\d|([1-9]\d+))(\.\d+)?g/;
       //不是空正则 只是一个单行注释而已；
```
- ES6中新增一特殊类型，Symbol，代表的是唯一的值;
```javascript
//需要不能更改的唯一值的时候，可以这样定义：
const a = Symbol('lalala');
```

<h4>扩展：js代码如何额被运行以及运行后如何输出结果？</h4>
<h5>【如何被运行】</h5>
- 把代码运行在浏览器中(浏览器内核来渲染解析)
- 基于NODE来运行（NODE也是一个基于V8殷勤渲染和解析js的工具）

<h5>【如何输出结果】</h5>
- alert (弹框输出，浏览器提示框) -> 基于alert输出的结果都会转化为字符串；
- confirm  和alert的用法一致，输出结果也是字符串只不过提示的内容有确定和取消两个按钮，叫确认提示框；
- prompt 在conmfirm上增加输入框
- console.log  在浏览器的控制台输出日志
    + Elements:当前页面中的元素和样式在这里都可以看到，还可以提调节样式修改调结构等等
    + console: 控制台，输出日志或者直接编写js代码
    + sources: 当前网站的源文件
- console.dir  比log更详细一点，尤其是输出对象数据的时候
- console.table  把一个json数据按照表格的格式输出

----

<h2>数据类型的详细剖析</h2>
1. number数据类型
   NaN：它是一个数据类型！！！
   isNaN：检测当前值是否为无效数字

<!-- '13' -->
<!-- true -->
<!-- false -->
<!-- null -->
<!-- [12] -->

<h3>重要： isNaN的监测机制</h3>
1. 首先验证当前检测的值本身就是数字类型的，如果不是，浏览器会吧默认的值转换为数字类型；
     把非数字类型的值转换为数字
- 其他类型的值转化为数字，直接使用Number方法转化，如果当前值中出现任意一个非有效数字字符结果则为NaN;

```javascript
[字符串转数字]
 Number('12') //12
 Number('12px') //NaN
 Number('12.5') //12.5
[布尔转数字]
Number(true) //1
Number(false) //0
[其他]
Number(null) //0
Number(undefined) //NaN
```
-  引用类型转化为数字： 先把引用类型的调取toString方法转化为字符串，然后在用Number方法对应的转化；
    【对象】
    ({}).toString --> '[object Object]'调取Number --》NAN

    Number（''） -> 0
    Number（[]） -> 0
    [].toString() --> '' --> isNaN([]) -->false
2. 检测当前值是有效数字，返回false，不是返回true（数字类型中只有NaN不是有效数字其余都是有效数字）；

----

2. parseInt / parseFloat
> 等同于Number，也是为了把其他类型的值转换为数字类型
>
> 区别在于字符串转化分析上
> Number：出现任意非有效数字字符，结果就是NAN
> parseInt：把一个字符串的整数部分解析出来
> parseFloat：把一个字符串的小数或者浮点数部分解析出来
```javascript
parseInt('13.5px') => 13
parseFloat('13.5px') => 13.5
parseFloat('width:13.5px') => NaN  字符查找从左向右，只要遇到非有效字符则结束查找，所以为你NaN
```
3. **NaN的比较，他和谁都不相等，包括他自己**

此条件永远不成立 因为NaN和任何都不相等：
    var a = XXX;
    if(Number(a) == NaN) {}

检测是否为有效数字只有用isNaN：
    var a = XXX;
    if(isNaN(a)) {}


