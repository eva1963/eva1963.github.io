---
title: Jquery源码分析
date: 2016-11-21 19:12:22
categories: JQUERY
tags: jquery,js
---

1.
```
//声明过得不会再次声明
console.log(foo);

function foo(){
console.log("foo");
}

var foo = 1;
```

2. [一元正号](undefined) (+)

   一元正号运算符位于其操作数前面，计算其操作数的数值，如果操作数不是一个数值，会尝试将其转换成一个数值。 尽管一元负号也能转换非数值类型，但是一元正号是转换其他对象到数值的最快方法，也是最推荐的做法，因为它不会对数值执行任何多余操作。它可以将字符串转换成整数和浮点数形式，也可以转换非字符串值 `true`，`false` `和` `null`。小数和十六进制格式字符串也可以转换成数值。负数形式字符串也可以转换成数值（对于十六进制不适用）。如果它不能解析一个值，则计算结果为 [NaN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN).

3. 判断类型方法

```
// 判断一个参数是什么类型的
var class2type = {};

// 生成class2type映射
"Boolean Number String Function Array Date RegExp Object Error".split(" ").map(item => {
  class2type["[object " + item + "]"] = item.toLowerCase()
});
// JQ使用自己的each写的
// jQuery.each("Boolean Number String Function Array Date RegExp Object Error".split(" "), function(i, name) {
//     class2type["[object " + name + "]"] = name.toLowerCase();
// });

function type(obj) {
  // 一箭双雕
  if (obj == null) {
      return obj + "";
  }
  return typeof obj === "object" || typeof obj === "function" ?
      class2type[Object.prototype.toString.call(obj)] || "object" :

      typeof obj;
}
// console.log(type([1, 2, 3]));
console.log(class2type);
```

4. `var length = "length" in obj && obj.length`

5. 判断是否是数组：

```javascript
isArray: Array.isArray || function(obj) {
   return jQuery.type(obj) === "array";
}
//原生有一个自带的isArray方法，然后在做兼容
```



