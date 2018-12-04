---
title: express
date: 2018-03-05 09:06:40
categories: NODE
tags: node,express,js
---

1. 当客户端向服务器端发送请求，如果请求方式是get，请求路路径是'/getUser'，就会把回调函数触发执行，里面有三个参数REQ，RES，next、

- req  不是之前原生的req，它是express框架封装处理的，也是存储了很多客户端传递信息的对象
  - **req.params**：存储的是路径参数，
  - **req.path**：请求的路径名称
  - **req.query**：get请求的问号传参信息，已经处理为对象了
  - **req.body**： 当请求的方式是POST，我们基于body-parser中间件处理后，会把客户端请求主体中传递的内容存放到body属性上
  - req.session： 我们给予express-session中间件处理后，会把客户端传递的session信息存放到这个属性
  - req.cookies: 我们给予cookie-parser中间件处理后，会把客户端传递的cookie信息存放到这个属性上
  - **req.get()**: 获取指定的请求头信息
  - **req.param()**； 基于这个方法，可以把url-encoded格式的字符串或者路径参数中的某一个属性名对应的信息获取到
- res也不是之前的原生node的res，也是经过处理的，目的是为了提供一些属性和方法，可以攻服务器向客户端返回内容；
  - res.cookie()通过此方法可以设置一些cookie信息，通过响应头set-cookie返回给客户端，客户端把返回的cookie信息种到本地；
  - res.json()向客户端返回一个json格式的字符串，但是允许我们传递json对象，方法会帮我们转换为字符串然后再返回（执行此方法后会自动结束响应，也就自动执行了res.end()）；
  - res.redirect()响应是重定向的，状态码是302；
  - res.render()只有页面是需要服务器渲染的时候我们才会用这个；
  - res.sendStatus:设置返回的状态码（它会结束响应，把状态码对应的信息，当做主体返回，我们一般都用status，然后自己来设置响应主体内容）
  - res.status()设置相应的状态码
  - res.type()设置相应内容的mime类型
  - res.sendFile(path);//把path指定的文件中内容得到，然后把内容返回给客户端（完成了文件读取和响应两步操作），也会自动结束响应
  - res.send()你想返回啥随便，是一个综合体
  - res.set({})//设置响应头

2. get请求的问号传参内容，可以使用req.query/req.param()
3. post请求，将请求主体传递的信息，此时我们需要一个中间件（body-parser）



#### express的中间件

> express里面的中间件，在API接口请求处理之前，把一些公共部分进行提取，中间件终究是先处理这些公共的部分，处理完成后，再继续执行接口请求即可

```javascript
app.use('/user',(req,res,next)=>{
  //请求的path地址中是以"/XXX"开头的，例如'/user','/user/add'...
  next();//不执行next是无法走到下一个中间件或者请求中的；
})
app.use((req,res,next)=>{
//  所有的请求都会走这个中间件，中间件的执行循序是按照书写的先后顺序执行
  next();
})
```

发送主体里面只能是字符串或者是buffer格式，所以我们POST发送的时候要把请求主体变成字符串传过去，在服务器后台获取的时候我们再把这个字符串处理为对象，进行下一步操作，然后返回给客户端的时候再转换为字符串，转来转去~

```javascript
// 如果是POST或普通请求，会把基于请求主体传递的信息预先截获
//如果传递的是url-encoded的字符串，基于urlencoded()把它传唤为对象键值对的方式
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: false}));
//这个中间件是供我们后续操作session的(理解为session在服务器端存储的时间)当中间件执行完成后会在req挂载一个session的属性，用来操作session
app.use(session({
    secret: 'evaf',
    saveUninitialized: false,
    resave: false,
    cookie: {maxAge: 1000*60*60*24)30}
}))

req.session.XXX ='sss';//设置session
req.session.XXX //获取session的值
```


