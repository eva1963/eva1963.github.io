---
title: CommonJS及其内置模块的使用
date: 2016-07-06 19:03:19
categories: NODE
tags: node,js,commonJS
---

1. npm是从npm官网下载包的；
2. yarn
3. bower是从github上面下载安装包的；

#### 在本地项目当中基于npm/yarn安装第三方模块

```
1. 在本地项目中创建一个package.json的文件，目的是把当前项目所有依赖的第三方模块信息(包含模块名称以及版本信息)都记录下来，可以在这里配置一些可执行的命令脚本等
2. 安装
3. 跑环境，部署的时候
```

##### package.json

基于yarn安装会默认生成一个package.json，只是信息没有手动创建的全面，所以我们一般都手动创建：

`npm init -y`		//加Y是为了一路都走默认值，无需询问

**意义：**开发项目的时候我们首先生成一个package.json，当我们安装第三方模块的时候把安装的模块信息记录到配置清单当中，这样以后不管是团队协作开发还是项目部署上线，我们都没有必要上传node_modules大文件上传，只需要上传配置清单，别人在使用的时候在自己的电脑中跑下环境即可

创建配置清单的时候不能出现中文，这样有可能识别不了

##### 安装

开发依赖：只有在项目开发阶段依赖的第三方模块

生产环境：项目部署实施的时候，也需要依赖的第三方模块

npm install xxx —save 	//保存到配置清单的生产依赖中

npm install XXX —save-dev	//保存到配置清单的开发依赖中

yarn add XXX 	//默认安装到生产依赖中

yarn add XXX —dev/-D	//安装到开发依赖中

##### 跑环境

直接**npm install /yarn install**，npm会自己先检测目录中是否有package.json文件，如果有就会按照文件中的配置清单依次安装；


#### 安装到本地和全局的区别：

安装到全局的特点：

- 所有的项目都可以使用这个模块，容易导致版本冲突
- 安装在全局的模块，不能基于CommonJS模块规范调取使用


安装到本地的特点：

- 只能当前项目使用这个模块
- 可以基于CommonJS模块规范调取使用
- 不能直接使用命令操作，全局的可以使用命令



为什么安装在全局下可以使用命令？

npm root /-g 查看本地项目或者全局环境下，npm的安装目录

**nvm切换node版本；（nvm ls;nvm use 8;）**

安装在全局目录下的模块，大部分都会生成一个XXX.cmd的文件，只要有这个文件，那么XXX就是一个可以执行的（例如：yarn.cmd=>yarn就是命令）

既可以安装在本地，也可以使用命令操作：

1. 配置package文件中的执行脚本
2. 把模块安装在本地，如果是支持命令操作的（会在node_modules的bin中生成一个xxx.cmd的命令文件，只不过这个文件无法在全局下执行，所以不能直接用命令）
3. 安装完成后，在package.json中的scripts中配置需执行的命令脚本，“eva”:"lessc -v"属性名自己设置即可，属性值是需要执行的命令脚本，根据需要自己编写即可（可以配置很多命令）
4. npm  run  eva/yarn eva这样的操作就是把配置脚本执行

- 首先到配置清单中的script中查找
- 找到把后面对应的属性值（脚本）执行
- 执行脚本的时候回到node_modules中的bin文件中查找,没有的话，再向npm安装的全局目录中去找

### node入门

node本身就是基于CommonJS模块规范设计的，所以模块是node的组成

- 内置模块：NODE天生提供给js调取使用的
- 第三方模块：别人写好的，我们可以基于npm安装使用
- 自定义模块：自己创建的一些模块

CommonJs模块化设计思想（AMD/CMD/ES6 MODULE都是模块设计思想）

- **规定每一个JS都是一个单独的模块**===>模块是私有的，里面涉及的值和变量以及函数等都是私有的，和其他的js文件是不冲突的
- CommonJS 中可以允许模块中的方法互相调用；
  - B模块想导入A模块的方法
    - A导出
    - B导入

【导出】：

CommonJS给每一个模块都设置了内置的变量/属性/方法

- **module：代表当前这个模块对象**
- **module.exports：提供的一个属性**，用来导出属性方法的
- **exports：是内置的变量**，也是用来导出当前模块属性方法的，虽然和module.exports不是一个东西，但是他俩代表的值是同一个，值都是对象（module.exports === exports）,指向的是同一个堆内存地址

【导入】：

**require**： CommonJS提供的内置变量，用来导入模块（其实导入的就是module.exports暴露出来的东西，导入的值也是[object]类型的）

**require是一个同步操作**： let temp1 = require('./temp1.js')；首先是把temp1中的代码从上到下执行一遍，吧exports对应的对内存导入进来，只有把导入的模块代码执行完成，才可以获取值，然后继续执行本模块下面的代码；

### CommonJS特点

1. **所有代码都运行在模块作用域**，不会污染全局作用域（每一个模块都是私有的，包括里面的东西都是私有的，不会和其他模块产生干扰）
2. **模块可以多次加载，但是只会在第一次加载时运行一次**，然后运行结果就被缓存了，以后在加载，就直接地取缓存结果，要想让模块再次运行，必须清理缓存；**为了保证性能**，减少模块代码重复执行的次数
3. 模块加载的顺序，按照其在在代码中出现的循序；**CommonJS规范加载模块是同步的**，也就是说，只有加载完成，才能执行后面的操作，而AMD是非同步的

默认和module.exports是同一个堆内存，但是exports= {}是让exports指向一个新的内存，module.exports不受影响



require('XXX')

require('XXX'),首先到当前项目node_modules下去找，不存在去找node提供的内置模块（导入第三方或者内置的）

__dirname:表示当前模块所在的绝对路径（物理路径）；

\_\_filename: 相对于\_\_dirname来说，多了模块名称；



### 内置模块：

#### FS[实现I/O操作]

let fs = require('fs');

**fs.readdir/readdirSync**:读取文件夹操作

```
fs.readdirSync('./');    //同步读取
fs.readdir('./', (err,files) => {
    err ? console.log(err) : null;
    console.log(files); //返回的结果是一个数组[ 'a.js', 'b.js', 'c.js', 'fs.js', 'js' ]
});
```

**fs.mkdir/fs.mkdirSync：创建文件夹/同步创建**，带sync是同步，不带就是异步操作，想要是先无阻塞的I/O操作，我们一般都是用异步操作来完成要处理的事情

```
// 读取文件中的内容
fs.readFile();
fs.writeFile(); //向文件中写入内容（覆盖写入：写入新的内容会替换原有的内容）
fs.appendFile();//追加写入新内容，原有内容还在
fs.watchFile();
fs.copyFile();  //拷贝文件到新的位置
fs.unlink();//删除文件
fs.access:用来验证权限
fs.rmdir();  //如果目录有文件，则不能直接删除
fs.rmdirSync();*/
```

#### PATH模块

#### URL模块

```
let url = require('url');

console.log(url.parse('http://www.zhufengpeixun.cn/main/guide/index.html?from=qq&ls=stu#video', true));

```

parse()第二个参数设置为true：
query参数是一个对象，里面是对象键值对的模式，常用的都是加true 

```
 Url {
 protocol: 'http:', 协议
 slashes: true, 是否有双斜线
 auth: null,作者
 host: 'www.zhufengpeixun.cn',域名+端口
 port: null,端口
 hostname: 'www.zhufengpeixun.cn',  域名
 hash: '#video',    //hash值
 search: '?from=qq&ls=stu', 问号传参
 query: 'from=qq&ls=stu',   问号传参的不带问号
 pathname: '/main/guide/index.html', 请求资源的路径名称
 path: '/main/guide/index.html?from=qq&ls=stu',
 href: 'http://www.zhufengpeixun.cn/main/guide/index.html?from=qq&ls=stu#video' }
 
```

#### HTTP模块

```
let http = require('http');
let server = http.createServer();//创建服务
server.listen();//监听端口号
```



对应好协议域名，端口等信息，



http:localhost:8080/...服务在电脑上，localhost本机域名了，也就是本机的客户端浏览器，访问本机的服务端程序

http://ip:8686/...

IP做域名访问，如果是内网IP，相同局域网下的用户可以访问这个服务，如果是外网IP，所有能联网的基本上都可以访问这个服务,局域网下访问需要互相关掉防火墙



#### createServer

- 当服务创建成功，并且客户端相当前服务器发送了请求，才会执行回调函数，并且发送一次请求，回调函数会被处罚执行一次
- 服务器上有一堆项目代码，这对代码既可能有服务端的程序代码，也有可能有客户端的程序代码，而客户端程序代码一般我们都放到static这个文件夹中
- 我们创建的web服务，需要处理两类请求，第一类属于静态资源文件的请求处理，第二类属于API 接口的请求处理，一个是要文件，一个是要数据，区别在于是否有后缀名，

**req.url**请求资源的路径地址及问号传参,不包含hash值

**req.methods** 客户端的请求方式，如GET

**req.headers**客户端的请求头信息，是一个对象

**res.write**基于这个方法服务器端可以向客户端返回内容

**res.end**结束响应

**res.writeHead**重写响应头信息

```
eg: res.end('hello word');服务器端返回给客户端的内容一般都是String或者Buffer格式的数据
```

如果是JSON格式的，使用JSON.stringify()转成json格式的字符串；

如果响应主体是汉字，做处理：

```
res.writeHead(200, {

        'content-type': 'text/plain;charset=utf-8'

    });

    res.end('我我我');
```

```
let { pathname, query } = url.parse(req.url, true);
console.log(pathname, query); 
```

```
let { url, method, headers } = req;
let { url, method, headers } = res;
console.log(url, method, headers);
console.log(req);
console.log(res);
```