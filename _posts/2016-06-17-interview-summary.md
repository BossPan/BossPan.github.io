---
title:      Interview Summary
summary:    问题和经验
categories: Interview
tags:       关于面试
---

2016/06/17

第一次面试，很紧张，很多简单基础性的问题都没答好，记得当时问了 jQuery 如何选择元素，就硬是没想起来，真是无语凝噎！同时这也说明自己基础不扎实，没有敲过的代码就回答不好。回忆一下部分面试题：

- 严格模式

- [] 和 new Array() 对比

- 阐述一下调用构造函数的过程

- className classList

- Ajax 有哪些应用

- JSONP 原理、缺点
  + `<script>` 标签获取数据，src 为服务器地址，服务器返回以回调函数包裹的一段  javascript 代码
  + 只能用于GET

- JSONP 和 Ajax 的关系

- 跨域解决方案有哪些 
  + CORS(H5 Access-Control-Allow-Origin)
  + postMessage
  + iframe

- 响应式布局 Media Query
  + IE 8 及以下不支持

```
@media screen and (min-width:600px) {
  nav {
    float: left;
    width: 25%;
  }
  section {
    margin-left: 25%;
  }
}
@media screen and (max-width:599px) {
  nav li {
    display: inline;
  }
}
```

- IE 8 兼容性问题
  + string.trim 函数
  + opacity

- !important 和标签内 style 的优先级
  + !important > 标签内 style

- insertBefore
  + 父元素调用，(要插入的元素，父元素的子元素)

- splice

- jQuery 选择器

- jQuery 延迟对象

- 有哪些影响网页加载速度的因素

- 图片预加载

- cookie 和 Web Storage 的区别

- 类继承的几种方法的对比

- 页面上有多个 Ajax 请求，如何判断所有请求都已经完成

- 动态文本如何进行垂直居中

- 阐述一下“声明提前”
  + http://www.tuliang.org/js-bianliang-shengmingtiqian/

- 有没有关注过什么新技术、博客

- 都学过哪些框架

- 有没有读过 jQuery 的源码，有什么收获

- 平常都通过什么方式学习