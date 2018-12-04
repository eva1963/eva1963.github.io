---
title: 初识lodash
date: 2018-05-08 09:29:11
categories: lodash
tags: lodash
---

最近在公司的项目中发现了lodash插件，因为之前也没有使用过underscore.js这样的插件库，所以一直对其没有研究过，不过既然项目中已经用到了，那就必须来学习一下，这里是我在项目中看到的部分，然后根据lodash官网的解释来记录的，所以可能不是很全，如果想要全面了解的，还是建议去官网学习哦~locash官网[http://www.css88.com/doc/lodash/]
在学习之前我们先来了解一下lodash是一个什么东东呢？

下面是来自简书同学的解释，地址：https://www.jianshu.com/p/d46abfa4ddc9
> “Lodash是一个著名的javascript原生库，不需要引入其他第三方依赖。是一个意在提高开发者效率,提高JS原生方法性能的JS库。简单的说就是，很多方法lodash已经帮你写好了，直接调用就行，不用自己费尽心思去写了，而且可以统一方法的一致性。Lodash使用了一个简单的 _ 符号，就像Jquery的 $ 一样，十分简洁。
> 类似的还有Underscore.js和Lazy.js”

使用起来也是非常方便，直接npm i lodash --save 即可

以下是项目中比较常见的方法：
引入项目中：`import _ from "lodash";`

`_.trim([目标字符串], [指定字符])`
> 指定字符默认为空格，所以只写第一个参数意思就是去空格，如果要去除指定的字符，就用到了第二个参数


`_.pick(object, [props])`
> 创建一个从 object 中选中的属性的对象。`_.omit`作用正好与之相反
```javascript
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.pick(object, ['a', 'c']);
// => { 'a': 1, 'c': 3 }
```

`_.values(object)`
> 创建 object 自身可枚举属性的值为数组
```javascript
function Foo() {
  this.a = 1;
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.values(new Foo);
// => [1, 2] (无法保证遍历的顺序)
 
_.values('hi');
// => ['h', 'i']
```

`_.assign(object, [sources])`
> 分配来源对象的 **可枚举属性** 到目标对象上。 来源对象的应用规则是从左到右，随后的下一个对象的属性会覆盖上一个对象的属性
```javascript
function Foo() {
  this.a = 1;
}
 
function Bar() {
  this.c = 3;
}
 
Foo.prototype.b = 2;
Bar.prototype.d = 4;
 
_.assign({ 'a': 0 }, new Foo, new Bar);
// => { 'a': 1, 'c': 3 }
```

`_.uniq(array)`
> 数组去重；创建一个去重后的array数组副本
```javascript
_.uniq([2, 1, 2]);
// => [2, 1]
```

`_.sortBy(collection, [iteratees=[_.identity]])`
> 创建一个元素数组。 以 iteratee 处理的结果升序排序
```javascript
var users = [
  { 'user': 'fred',   'age': 48 },
  { 'user': 'barney', 'age': 36 },
  { 'user': 'fred',   'age': 40 },
  { 'user': 'barney', 'age': 34 }
];
 
_.sortBy(users, function(o) { return o.user; });
// => objects for [['barney', 36], ['barney', 34], ['fred', 48], ['fred', 40]]
 
_.sortBy(users, ['user', 'age']);
// => objects for [['barney', 34], ['barney', 36], ['fred', 40], ['fred', 48]]
 
_.sortBy(users, 'user', function(o) {
  return Math.floor(o.age / 10);
});
// => objects for [['barney', 36], ['barney', 34], ['fred', 48], ['fred', 40]]
```

`_.clone(value)`
> 创建一个value的浅拷贝
```javascript
var objects = [{ 'a': 1 }, { 'b': 2 }];
 
var shallow = _.clone(objects);
console.log(shallow[0] === objects[0]);
// => true
```

`_.cloneDeep(value)`
> 这个方法类似_.clone，除了它会递归拷贝 value
```javascript
var objects = [{ 'a': 1 }, { 'b': 2 }];
 
var deep = _.cloneDeep(objects);
console.log(deep[0] === objects[0]);
// => false
```

`_.findIndex(array, [predicate=_.identity], [fromIndex=0])`
> 该方法类似_.find，区别是该方法返回第一个通过 predicate 判断为真值的元素的索引值（index），而不是元素本身
```javascript
var users = [
  { 'user': 'barney',  'active': false },
  { 'user': 'fred',    'active': false },
  { 'user': 'pebbles', 'active': true }
];
 
_.findIndex(users, function(o) { return o.user == 'barney'; });
// => 0
 
// The `_.matches` iteratee shorthand.
_.findIndex(users, { 'user': 'fred', 'active': false });
// => 1
 
// The `_.matchesProperty` iteratee shorthand.
_.findIndex(users, ['active', false]);
// => 0
 
// The `_.property` iteratee shorthand.
_.findIndex(users, 'active');
// => 2
```

`_.flatten(array)`
> 减少一级array嵌套深度
```javascript
_.flatten([1, [2, [3, [4]], 5]]);
// => [1, 2, [3, [4]], 5]
```

`_.flattenDeep(array)`
> 将array递归为一维数组
```javascript
_.flattenDeep([1, [2, [3, [4]], 5]]);
// => [1, 2, 3, 4, 5]
```

    查看lodash官方文档后你会发现，其实这个就是进化后的JQuery，lodash提供了更强大的函数工具给我们使用，大大的方便了我们的开发，提高了我们的而开发效率，所以了解lodash是必要的，如果你还有精力的话，也建议你去看看lodash的源码，就想之前看jquery的源码一样，相信了lodash的源码也不会让你失望，会使你大大受益~
    好了，就说到这里了，工作~

