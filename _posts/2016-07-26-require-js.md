---
title:      RequireJS 入门
summary:    JavaScript 模块化、RequireJS
categories: note javascript
tags:       JavaScript RequireJS
---

原始需求是这样的：每个页面都需要引入好几个JS文件，每个文件之间存在依赖，并且其中有一些是相同文件，我们知道这样会增加HTTP请求次数，减慢网站的加载速度，所以我需要一种类似于sass的import语法的解决方案。

### 为什么需要模块化

- 网站正在变成网络应用，代码复杂度随着网站变大而增加，代码组织变难了
- 开发者希望 JS 文件模块化
- 部署时又希望将代码优化进一到数次 HTTP 请求

### JavaScript 模块化进程

从无模块化时代 -> 模块萌芽（自执行函数、YUI命名空间、jQuery匿名自执行函数）-> 源自Node.js的CommonJS -> Modules/Transport、RequireJS(AMD)、SeaJS(CMD) -> 面向未来的ES6模块标准

### RequireJS 入门

1.使用RequireJS

首先需要在页面中引入require.js。可以从 [DOWNLOAD REQUIREJS](http://requirejs.org/docs/download.html) 下载。

```
<script data-main="js/app.js" src="js/require.js"></script> // data-main用于指定程序入口文件
```

在app.js中就可以使用模块化的方式书写，比如指定依赖。

2.合并与压缩

使用require指定依赖并不能减少HTTP请求的次数，每个模块是单独加载的，但RequireJS提供了[Optimization](http://www.requirejs.cn/docs/optimization.html) 来减少加载时间。

合并与压缩文件

```
node r.js -o baseUrl=. paths.jquery=some/other/jquery name=main out=main-built.js
```

或者将配置写入build.js，build.js的配置可以参考[example.build.js](https://github.com/requirejs/r.js/blob/master/build/example.build.js)

```javascript
// build.js
({
    baseUrl: ".",
    name: "main",
    out: "main-built.js"
})

node r.js -o build.js
```

以上方法不会压缩require.js，下面这个命令可以将 require.js 文件一同合并。

```
r.js -o baseUrl=. paths.requireLib=./require name=main include=requireLib out=main-built.js
```

### 参考目录

- [js模块化历程](http://www.cnblogs.com/lvdabao/p/js-modules-develop.html)
- [为何用 Web 模块的方式？](http://cyj.me/why-seajs/requirejs/)
- [REQUIREJS API](http://requirejs.org/docs/api.html)
- [RequireJS中文网](http://www.requirejs.cn/)

