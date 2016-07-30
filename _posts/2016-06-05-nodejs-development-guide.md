---
title:      Node.js开发指南
summary:    摘录书中一些知识点。
categories: JavaScript Node.js
tags:       JavaScript Node.js 学习笔记
---

## Node.js 入门

### 1. Node.js 简介

- Node.js 是一个让 Javascript 运行在服务器端的开发平台。
- Node.js不运行在浏览器中，不存在 JavaScript 的浏览器兼容性问题，你可以放心地使用 JavaScript 语言的所有特性。
- Node.js 最大的特点是单线程、异步式 I/O、事件驱动的程序设计模型。
- 适合用来做 I/O 调度，不太适合用来做复杂计算。
- [Node.js 优缺点及适用场景讨论](http://www.cnblogs.com/sysuys/p/3460614.html)

### 2. 安装和配置

- [https://nodejs.org/en/](https://nodejs.org/en/)

### 3. Node.js 快速入门

#### 3.1 建立 HTTP 服务器

```
var http = require('http');

http.createServer(function(req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write('<h1>Node.js</h1>');
    res.end('<p>Hello World</p>');
}).listen(3000);

console.log("HTTP server is listening at port 3000.");
```

#### 3.2 模块

- 文件和模块一一对应。
- exports是模块统一的接口。
- 创建模块。

```
var name;

exports.setName = function(thyName) {
    name = thyName;
};

exports.sayHello = function() {
    console.log('Hello ' + name);
};
```
- 获取接口。

```
var myModule = require('./module');

myModule.setName('BYVoid');
myModule.sayHello(); //'Hello AYVoid'
```
- require 不会重复加载模块。（Node.js 通过文件名缓存所有加载过的文件模块）

``` JavaScript
var hello1 = require('./module');
hello1.setName('BYVoid');

var hello2 = require('./module');
hello2.setName('BYVoid 2');

hello1.sayHello(); //'Hello BYVoid 2'
```
- 覆盖 export，将对象封装到模块中(事实上，exports 只是一个普通的空对象)。

```
function Hello() {
    var name;

    this.setName = function (thyName) {
        name = thyName;
    };

    this.sayHello = function () {
        console.log('Hello ' + name);
    };
};

exports.Hello = Hello;
```

#### 3.3 包

- 模块和包没有本质区别，包可以理解成是实现了某个功能模块的集合。
- 创建包。
    - 作为文件夹的模块
    - packpage.json

```
//somepackage/index.js

exports.hello = function() {
    console.log('Hello.');
};

//getpackage.js

var somePackage = require('./somepackage');
somePackage.hello();
```
Node.js 在调用某个包时，会先检查 packpage.json 文件中的 'main' 字段，将其作为包的接口模块。

### 4. 调试

- 命令行调试

```
$ node debug dubug.js
```
- 远程调试

```
//在一个终端中
$ node --debug-brk[=port] srcript.js

//在另一个终端中
$ node --node debug 127.0.0.1:5858
```
- 调试工具 node-inspector

```
$ node-inspector
```

```
$ node --debug-brk[=port] srcript.js
```

## Node.js 核心模块

- 全局对象；
- 常用工具；
- 事件机制；
- 文件系统访问；
- HTTP 服务器与客户端。

### 1. 全局对象
- Node.js 的全局对象是global
- 全局变量 process
- 全局变量 console

### 2. 常用工具 util
- util 提供常用函数的集合。
- util.inherits(constructor, superConstructor) 是一个实现对象间原型继承
的函数。
- util.inspect(object,[showHidden],[depth],[colors])是一个将任意对象转换为字符串的方法，通常用于调试和错误输出。

### 3.事件驱动 events
- events 模块只提供了一个对象： events.EventEmitter。
- EventEmitter.on/emit/once/removeListener/removeAllListener
- error 事件。
- 继承 EventEmitter，大多数时候我们不会直接使用 EventEmitter，而是在对象中继承它。

### 4.文件系统 fs
- fs 模块中所有的操作都提供了异步的和同步的两个版本。
- fs.readFile(filename,[encoding],[callback(err,data)])。

```
//readfileencoding.js

var fs = require('fs');

fs.readFile('content.txt', 'utf-8', function(err, data) {
    if (err) {
        console.error(err);
    } else {
        console.log(data);
    }
});
```

- fs.readFileSync(filename, [encoding])是 fs.readFile 同步的版本。
- 与同步 I/O 函数不同，Node.js 中异步函数大多没有返回值。
- fs.open(path, flags, [mode], [callback(err, fd)])。
- fs.read(fd, buffer, offset, length, position, [callback(err, bytesRead,
buffer)])是 POSIX read 函数的封装，相比 fs.readFile 提供了更底层的接口(除非必要，否则不要使用这种方式读取文件)。

### 5. HTTP 服务器与客户端

#### 5.1 HTTP 服务器
- http.Server 是 http 模块中的 HTTP 服务器对象。
- http.Server 的事件：request、connection、close。
- 最常用的事件是 request，因此 http 提供了捷径： http.createServer([requestListener])

```
//httpserver.js

var http = require('http');

var server = new http.Server();
server.on('request', function(req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write('<h1>Node.js</h1>');
    res.end('<p>Hello World</p>');
});
server.listen(3000);

console.log("HTTP server is listening at port 3000.");
```

- http.ServerRequest 是 HTTP 请求的信息，包括data、end、close 事件。
- http.ServerResponse 是返回给客户端的信息，有三个重要的成员函数response.writeHead(statusCode, [headers])、response.write(data, [encoding])、response.end([data], [encoding])。

#### 5.2 HTTP 客户端
- http 模块提供了两个函数 http.request 和 http.get，功能是作为客户端向 HTTP 服务器发起请求。
- http.request(options, callback) ，记得要手动调用 req.end(); 结束请求，否则服务器将不会收到消息
- http.get 自动将请求方法设置为 get，并且不需要手动调用 req.end()。
- http.ClientRequest 是由 http.request 或 http.get 返回产生的对象，表示一个已经产生而且正在进行中的 HTTP 请求。
- http.ClientResponse 与 http.ServerRequest 相似，提供了三个事件 data、end和 close，分别在数据到达、传输结束和连接结束时触发。

## 使用 Node.js 进行 Web 开发

### 1. MVC（Model-View-Controller，模型-视图-控制器）

- 模型是对象及其数据结构的实现，通常包含数据库操作。
- 视图表示用户界面，在网站中通常就是 HTML 的组织结构。
- 控制器用于处理用户请求和数据流、复杂模型，将输出传递给视图。

### 2. 搭建微博网站

- 首先我想说一下，由于 Node.js 版本不同，对着书上来写，一步步都是坑啊啊啊啊啊啊！
- 参考 [connect-middleware](https://github.com/senchalabs/connect#middleware "connect-middleware")

#### 2.1 安装 Express

```
$ npm install express
$ npm install -g express-generator
```

#### 2.2 创建以 ejs 为模板引擎的工程
```
$ express -e ejs  microblog
```

#### 2.3 启动服务器（先切换至microblog目录）,任选一个命令
```
$ node ./bin/www
$ supervisor ./bin/www
$ npm start
```

#### 2.4 网站结构说明
- 入口文件 app.js
- 路由文件（控制器）index.js
- 模板文件 index.ejs
- 页面框架 layout.ejs，默认被所有模板文件继承

![Express网站架构](/img/nodejs/structure-of-express.png)

#### 2.5 模板引擎
![模板引擎在MVC架构中的位置](/img/nodejs/model.png)

#### 2.6 片段视图 partial

```
var partials = require('express-partials');
app.use(partials());

<%- partial('listitem', items) %>
```

或者用include 代替

```
<% items.forEach(function(listitem){ %> <% include listitem %> <% }) %>
```

#### 2.7 视图助手
```
// 视图交互
app.use(function (req, res, next) {
    res.locals.user = req.session.user;
    res.locals.post = req.session.post;
    var error = req.flash('error');
    res.locals.error = error.length ? error : null;

    var success = req.flash('success');
    res.locals.success = success.length ? success : null;
    next();
});
```

#### 2.8 需要注意的地方
- title 改为 locals.title，user 同理
- Connection.DEFAULT_PORT 改为 27017
- 注意顺序，比如在 app.js 中 session 应该放在前面

## Node.js 进阶 

### 1. 模块加载

- 模块类型： .js > .json > .node
- 核心模块 > 文件模块
- 文件模块加载有两种方式：按路径加载/查找 node_modules 文件夹
- 查找 node_modules 文件夹的方式： 逐级向上查找，直到根目录位置

#### 模块加载顺序

1. 如果 some_module 是一个核心模块，直接加载，结束。
2. 如果 some_module 以“ / ”、“ ./ ”或“ ../ ”开头，按路径加载 some_module，结束。
3. 假设当前目录为 current_dir，按路径加载 current_dir/node_modules/some_module。
    - 如果加载成功，结束。
    - 如果加载失败，令current_dir为其父目录。
    - 重复这一过程，直到遇到根目录，抛出异常，结束。

### 2. 控制流

- 回调函数带来的问题

```
var fs = require('fs');
var files = ['a.txt', 'b.txt', 'c.txt'];

for (var i = 0; i < files.length; i++) {
    fs.readFile(files[i], 'utf-8', function (err, contents) {
        console.log(files);
        console.log(i);
        console.log(files[i]);
    });
}
```

### 3. 日志功能

```
// 访问日志
var fs = require('fs');
var accessLogStream = fs.createWriteStream(__dirname + '/access.log', {flags: 'a'});
var morgan = require('morgan');
app.use(morgan('combined', {stream: accessLogStream}));

// 错误日志
var errorLogStream = fs.createWriteStream(__dirname + '/error.log', {flags: 'a'});
app.use(function (err, req, res, next) {
    var meta = '[' + new Date() + '] ' + req.url + '\n';
    errorLogStream.write(meta + err.stack + '\n');
});
```

### 4. cluster 模块（利用多核资源）

- cluster 的功能是生成与当
前进程相同的子进程，并且允许父进程和子进程之间共享端口。
- [API](https://nodejs.org/docs/latest-v5.x/api/cluster.html)

### 5. 共享 80 端口

## 简单总结

记得是 5 月 28 号 拿起这本书的，今天已经是 6 月 8 号了，感觉时间过得好快，虽然其中有几天都是准备考试去了，但还是觉得自己学习的效率不高。如今到了真正找实习了才知道自己的技术有几斤几两，心里苦啊，好希望赶紧来个公司收了我！好吧回到正题...

从书中学到的东西：

- 什么是 Node.js 
- Node.js 的特点
- 模块和包，模块的加载顺序
- Node.js 核心模块
- 如何使用 Node.js + Web 开发框架（Express）建立一个微博系统，[点击查看代码](https://github.com/BossPan/microblog)
    - Mongodb 了解和使用
    - Mongodb 的可视化管理工具 Robomongo
- Node.js 的日志功能 
- Node.js 多核处理模块 cluster

以后有时间可以学习一下 [从零开始 nodejs 系列文章](http://blog.fens.me/series-nodejs/)

就到这儿吧！