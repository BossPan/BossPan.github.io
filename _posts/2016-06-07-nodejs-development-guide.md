---
layout:     post
title:      Node.js开发指南 / 学习笔记 04
summary:    Chapter 06 Node.js 进阶 
categories: note
---

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
fs.readFile(files[i], 'utf-8', function(err, contents) {
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
app.use(function(err, req, res, next) {
   var meta = '[' + new Date() + '] ' + req.url + '\n';
   errorLogStream.write(meta + err.stack + '\n');
});
```

### 4. cluster 模块（利用多核资源）

- cluster 的功能是生成与当
前进程相同的子进程，并且允许父进程和子进程之间共享端口。
- [API](https://nodejs.org/docs/latest-v5.x/api/cluster.html)

### 5. 共享 80 端口














