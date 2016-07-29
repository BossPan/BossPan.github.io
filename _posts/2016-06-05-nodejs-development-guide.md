---
layout:     post
title:      Node.js开发指南 / 学习笔记 03
summary:    Chapter 05 使用 Node.js 进行 Web 开发
categories: note
---

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



