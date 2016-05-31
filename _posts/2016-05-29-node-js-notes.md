---
layout:     post
title:      Node.js开发指南/学习笔记 02
summary:    Chapter 04
categories: notes
---
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

#### HTTP 服务器
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

#### HTTP 客户端
- http 模块提供了两个函数 http.request 和 http.get，功能是作为客户端向 HTTP 服务器发起请求。
- http.request(options, callback) ，记得要手动调用 req.end(); 结束请求，否则服务器将不会收到消息
- http.get 自动将请求方法设置为 get，并且不需要手动调用 req.end()。
- http.ClientRequest 是由 http.request 或 http.get 返回产生的对象，表示一个已经产生而且正在进行中的 HTTP 请求。
- http.ClientResponse 与 http.ServerRequest 相似，提供了三个事件 data、end和 close，分别在数据到达、传输结束和连接结束时触发。