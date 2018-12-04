---
title: 本地存储 VS 服务器存储
date: 2017-12-15 19:18:18
categories: webpack
tags: 前端，js，localStorage，session，session，cookie
---

本地存储：把一些信息存储到客户端本地（主要目的有很多，其中有一个就是实现多页面之间的信息共享）

- 离线缓存（XXX.mainfest）	H5处理离线缓存还是存在一些硬伤的，所以真实项目中一般还是传统的native APP来完成这件事情
- **localStorage/sessionStorage**:H5中新增的API，基于这个API可以把这些数据缓存到客户端本地（常用）
- IndexDB/webSQL：本地数据库存储
- cookie：本地存储，和localStorage差不多，比较常用
- CacheStorage、ApplicationCache：本地缓存存储

用到本地存储的地方:

**【页面之间信息的通信：包括登录注册】**

A存信息 ，B页面中可以使用；比如登录，记住用户密码，购物车，跳转其他页面再回来的时候还停留在上次的位置

- 比如记住密码功能：当我们手动完成一次登录注册的时候，下次打开这个网站的时候，首先查看本地信息中是否有用户名和密码，如果已经有了说明上一次记住密码了，这一次我们就直接拿本地存储中的用户名和密码发送服务器完成一次请求，完成一次登录操作，成功就直接进页面，失败了就返回登录页
- 下次重新打开登录页面，我们需要先从本地把信息获取到把信息放到对应的文本框中

需求：A页面有一个列表，点击列表中的每一项，跳转到B页面（详情页面），B页面需要知道点击的是A中的那一条数据，从而展示不同的信息；

URL问号传参： ‘B.html/id=XXX’

这种方式实现前提：需要A中的某个操作需要跳转到B的页面，此时才可以问号传参

- 本地存储都是存储到当前浏览器指定的地方，所以在google中存储的信息不能再IE中拿到，也就是说**本地存储无法跨浏览器进行传输**
- 存储的信息是按照于来管理的；访问JD网站把信息都存储到了JD.COM中，其他域的网站中是无法直接获取这些信息的，也就说**本地存储不能直接跨域访问；**

真实项目中的登录注册，是基于session服务器存储的登录信息，而不是本地存储（因为本地存储，不安全）首先是明文存储，直接在浏览器的控制台中就可以查看到

重要任务：如果是登陆成功，服务器会把用户的一些基本信息以及是否登录成功，在服务器端基于session存储起来（服务器存储）

把成功或者失败的结果返回给客户端

**【做一些性能优化】**

把一些不经常改变的数据，在第一次从服务器端获取到之后，存储到客户端本地（记录一个存储时间），假设我们设置的有效存储期是10分钟，那么10分钟以内，我们再刷新页面就不用在向服务器发送请求了直接从本地数据中获取展示即可；超过十分钟重新向服务器发送请求，请求回来最新数据参考第一次，也存储到本地中；

1. 减轻服务器压力
2. 对于不经常更新的数据我们可以把存储的周期设置的长一些，有助于页面第二次加载的时候渲染的速度（移动端经常做这些处理）
3. ​







#### session和cookie的关联

1. session是服务器存储，cookie是客户端存储
2. 在服务器上建立session之后，服务器和当前客户端之间会建立一个唯一的标识（sessionID），而本次存储的session信息都存放到对应的sessionID下，目的是为了区分不同客户端都在服务器上建立session信息，后期查找的时候能够找到自己当初建立的
3. 当服务器把一些成功或者失败的结果返回给客户端的时候，在响应头信息中增加cookie这样的字段，把sessionId存储到客户端的cookie当中，httpOnly规定当前的cookie信息只能获取使用，但是不能修改
4. 当客户端在向服务器发送请求的时候，在请求头当中都会把cookie信息带上传递给服务器，也包含之前存储的sessionID
5. 服务器响应头信息中增加set-cookie:seesionID
6. 在客户端本地中下cookie，再发送其他请求的时候再把cookie信息放到请求头中传递过去,服务器校验这个id是否正确

#### 加入购物车原型

没登录的时候：

1. 点击加入购物车，客户端把商品标识传过去，服务器建立session存储当前加入购物车信息（以后再加入购物车直接向这个容器中继续存储即可），存完返回给客户端成功还是失败，也把sessionID带过来给客户端了
2. 此时本地中已经有sessionID了，我们打开购物车列表页的时候，发送请求的时候浏览器会自动发送携带sessionID的cookie过去，这样服务器就知道返回那些购物车的信息给你了

登录的状态下:

1. 未登录状态下，加入购物车的信息都是session中存储，在登录后都要先保存到数据库当中，然后再把session信息清除
2. 数据库存储的目的就是为了实现购物信息的跨平台共享



#### localStorage vs cookie

**[cookie]**

1. 兼容所有的浏览器
2. 有存储的大限制，一般一个源里面只能存储4KB，url的长度是2KB（每个域名下的cookie 的大小最大为4KB，每个域名下的cookie数量最多为20个）
3. cookie有过期时间（当然我们自己可以手动设置这个时间）
4. 杀毒软件或者浏览器的垃圾清理都可能会把cookie强制清掉
5. 在隐私或者无痕浏览模式下，是不记录cookie的
6. cookie不是严格的本地存储，因为要和服务器之间来回传输



**[localStorage]**

1. 不兼容低版本浏览器
2. 也有存储的大小限制，一个源最多只能存储5MB左右；
3. 本地永久存储，只要不手动删除（但是我们可以基于API如removeIItem/clear手动清除一些想啊哟删除的信息）
4. 杀毒软件或者浏览器的垃圾清理暂时不会清除localStorage（新版本google会清除）
5. 在隐私或者无痕浏览模式下，是记录localStorage的
6. localStorage跟服务器毫无关系

真实项目中使用本地存储来完成一些需求的情况不是很多，一般都是基于服务器的session或者数据库存储完成的（服务器的session和本地的cookie是有关联的），如果不考虑兼容，只想基于本地存储来完成一些事情，那么一般都是用localStorage的（尤其是移动端开发）



### localStorage的API：

setItem/getItem/removeItem/clear/key(通过指定的索引获取指定的key名)



#### cookie设置

document.cookie = '';//设置cookie

封装的cookie

```
let cookie = (function () {
    let setValue = (name, value, expires = (new Date(new Date().getTime() + (1000 * 60 * 60 * 24))), path = '/', domain = '')=> {
        document.cookie = `${name}=${escape(value)};expires=${expires.toGMTString()};path=${path};domain=${domain}`;
    };

    let getValue = name=> {
        let cookieInfo = document.cookie,
            reg = new RegExp(`(?:^| )${name}=([^;]*)(?:;|$)`),
            ary = cookieInfo.match(reg);
        return ary ? unescape(ary[1]) : null;
    };

    let removeValue = (name, path = '/', domain = '')=> {
        let value = getValue(name);
        if (value) {
            document.cookie = `${name}= ;path=${path};domain=${domain};expires=Fri,02-Jan-1970 00:00:00 GMT`;
        }
    };

    return {
        set: setValue,
        get: getValue,
        remove: removeValue
    }
})();
```


