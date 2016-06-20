---
layout:     post
title:      Node.js开发指南 / 学习笔记 01
summary:    Chapter 01~03 Node.js 入门
categories: note
---
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

```
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


