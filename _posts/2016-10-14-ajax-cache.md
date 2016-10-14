---
title:      Ajax 缓存
summary:    Ajax 缓存控制，如何禁用缓存
categories: JavaScript Ajax
tags:       JavaScript Ajax 缓存
---

## 设置缓存

服务器控制缓存的三个 HTTP 响应首部为 Cache-Control、Expires、Last-Modified。

### Cache-Control

max-age 值定义了文档的最大使用期：从第一次生成文档到文档不再新鲜、无法使用为止，最大的合法

生存时间（以秒为单位）。`Cache-Control: max-age=484200`

### Expires

指定一个绝对的过期日期。如果过期日期已经过了，就说明文档不再新鲜了。`Expires: Fri, 05 Jul 2002, 05:00:00 GMT`

### Last-Modified

Last-Modified 响应头部和 If-Modified-Since 请求首部配合工作。大致原理是这样的：客户端首次请求服务器资源时，服务器的响应头部设置 Last-Modified 用来标识资源修改的时间，客户端记录这个时间。当客户端再次发起同样的请求时，请求头部中的 If-Modified-Since 设置为记录的时间，服务器通过请求头中的 If-Modified-Since 值做出判断。

如果 If-Modified-Since 指定日期的后，文档修改了，通常 GET 就会成功执行。携带新首部的新文档会被返回给缓存，新首部除了其他信息之外，还包含了一个新的过期日期。

如果自指定日期后，文档没被修改过，会向客户端返回一个小的 304 Not Modified 响应报文，为了提高有效性，不会返回文档的主体。这些首部是放在响应中返回的，但只会返回那些需要在源端更新的首部。比如，Content-Type 首部通常不会被修改，所以通常不需要发送。一般会发送一个新的过期日期。

## 禁用缓存

服务器在设置了缓存策略的情况下，一般的Ajax GET 请求是会被缓存的，如果需要禁用缓存，有以下几种方法。

## 添加时间戳

这是一种常见方法，在 GET 请求的 url 中添加一个时间戳，那么每个请求的 url 都是不一致的，就是一个新的请求，自然就没有缓存问题。

```javascript
url += 'timestamp=' + new Date().getTime();
```

## 设置请求头

可以通过给 XMLHttpRequest 对象设置缓存控制的头部来实现。

```javascript
http.setRequestHeader('Cache-Control', 'no-cache');
```

或者

```javascript
http.setRequestHeader('If-Modified-Since','0');
```

## jQuery 方法

参数对象中添加属性`cache: false` 。

```javascript
$.ajax({
    method: 'GET',
    url: url,
    cache: false,
    success: function () {}
});
```

事实上这种方法的原理就是添加时间戳，在 Chrome 里面通过 Network 查看请求的话，可以看到 url 尾部添加的是 `_=time` （time 是请求的时间）

## 修改服务器

修改服务器的缓存配置。

```php
header('Cache-Control: no-cache');
```

## 参考附录

- `HTTP 权威指南 第七章 缓存`