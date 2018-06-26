---
title: DOM盒子模型
date: 2017-07-26 15:17:09
categories: JS
tags: 前端
---

在js当中通过相关的属性可以获取（设置）元素的样式信息，这些属性就是盒子模型属性（基本上都是关于样式信息）
```css
client系列
- top
- left
- width
- height

offset系列
- top
- left
- width
- height
- 	parent


scroll系列
- top
- left
- width
- height
```
`clientWidth  & clientHeight`   只包括盒子的内容+padding，不包括边框
   1. 和内容是否有溢出无关，和是否设置了overflow: hidden;也无关，就只是我们自己设置的内容+padding
```javascript
获取屏幕的宽高：
document.documentElement.clientWidth || document.body.clientWidth;
document.documentElement.clientHeight || document.body.clientHeight;
```
`clientTop & clientLeft` : 获取（上/左）边框的宽度


`offsetWidth & offsetHeight`:在clientWidth的基础上加上border（和内容溢出也没有关系）

`scrollWidth & scrollHeight `:真实内容的宽高（
1. 不一定是自己的设置的宽高，因为可能存在内容溢出，有内容溢出的情况下，需要把溢出的也算在里面，而且值是一个约等于值，并不是那么准确
2. 没有内容溢出的时候和client值是一样的
3. 在不同的浏览器当中，是否设置了overflow都会对最后的结果产生影响，所以这个值仅做参考
   ）

```javascript
获取当前页面的真实宽高：
document.documentElement.scrollWidth || document.body.scrollWidth;
document.documentElement.scrollHeight || document.body.scrollHeight;
```

通过盒子模型获取的值的特点：
1. 都是数字，且不带单位
2. 获取的都是整数，且不会出现小数
3. 获取的结果都是复合样式值（content+padding等），如果你只想获取padding那js盒子模型无法操作，一般真实项目中我们都是需要获取复合值来完成一些操作的

---

获取元素具体的但单一样式：
1. xxx.style.yyy,如oBox.style.color,但这个只能获取所有写在行内元素上的样式，单独样式表中的获取不到，而真实项目中我们很少会把样式写在行内样式上
2. 获取当前元素所有经过浏览器计算过的元素
   > 经过计算的样式：经过浏览器渲染过的能在当前页面展示的，那它的样式就都是被计算过的
   > 不管当前样式写在哪，不管你写没写，他都有自己的默认样式

标准浏览器（IE9+）
`window.getComputedStyle([元素],[伪类，一般都写null]);`
但是在IE6-8中么有这个属性，他们都用currentStyle来获取经过计算的属性*/

`offsetParent`当前盒子的父级参照物
`offsetLeft/offsetTop`获取当前盒子距离其父级参照物的偏移量(计算：从当前盒子的外边框，到父级盒子的内边框)
```
	同一个平面中，元素的父级参照物和结构没有必然联系，默认他们的父级参照物都是body（当前平面最外层的盒子）body的父级参照物是null
	参照物是可以改变的，构建出不同的平面即可（使用z-index,但需要position定位），所以改变元素的定位可以改变其父级参照物
```

`scrollTop/scrollLeft`滚动条卷去的宽度或者高度
最小值0
最大值： 真实页面的高度减去一屏幕的高度**document.documentElement.scrollHeight-document.documentElement.clientHeight ||  document.body.scrollHeight-document.body.clientHeight **
操作浏览器的盒子模型属性，我们一般都写两套，用来兼容各种模式下的浏览器；
`在js盒子模型的十三个属性当中，只有scrollTop和scrollLeft是可读写属性；`其他的都是只读的