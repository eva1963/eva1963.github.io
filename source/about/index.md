---
title: about

HTML5中的localStorage对象
---

HTML5中提供了localStorage对象可以将数据长期保存在客户端，直到人为清除。
localStorage提供了几个方法:
1、存储：localStorage.setItem(key,value)
如果key存在时，更新value

2、获取：localStorage.getItem(key)
如果key不存在返回null

3、删除：localStorage.removeItem(key)
一旦删除，key对应的数据将会全部删除

4、全部清除：localStorage.clear()
某些时候使用removeItem逐个删除太麻烦，可以使用clear,执行的后果是会清除所有localStorage对象保存的数据

5、遍历localStorage存储的key
.length 数据总量，例：localStorage.length
.key(index) 获取key，例：var key=localStorage.key(index);

