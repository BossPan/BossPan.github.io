---
title:      客户端存储机制
summary:    在浏览器中保存数据, Cookie, Web Storage, IE userData
categories: JavaScript Cookie
tags:       JavaScript Cookie Web-Storage
---

## Cookie

cookie是一种早期的客户端存储机制，起初是针对服务器端脚本设计使用的。

cookie数据会自动在Web浏览器和Web服务器之间传输，因此服务端脚本可以读、写存储在客户端的cookie的值。

任何以cookie形式存储的数据，不论服务器端是否需要，每一次HTTP请求都会把这些数据传输到服务器端。

每个域的cookie总数有限，不同浏览器之间各不相同。浏览器对cookie的尺寸也有限制，大多数浏览器都有约4096B（加减1）的长度限制。

当超过cookie的长度限制时，再设置cookie将被舍弃。

### cookie的构成

1.名称

不区分大小写，需要经过URL编码。

2.值

需要经过URL编码。

3.域 domain

指定cookie生效的域。domain=".example.com"

4.路径 path

指定cookie生效的路径，访问指定域中的指定路径时，应该向服务器发送cookie。

5.失效时间 expires

表示cookie何时应该被删除的时间戳。默认情况下，如果没有为cookie设置该属性，则在浏览器关闭（注意不是标签页）时将cookie删除。设置了该属性的cookie，将在指定时间呗删除，如果设置的是一个过去的时间，则cookie将被立刻删除。

#### 安全标志 secure

指定后，cookie只有在使用SSL连接时才发送到服务器。

### 设置、读取、删除 cookie

一个实例。

```javascript
document.cookie = encodeURIComponent("location") + "=" + encodeURIComponent("Dalian") + "; expires=" +  new Date("2016-10-4") + "; path=" + "/" + "; domain=" + "bosspan.net;";
```

由于JavaScript中读写cookie不是很直观，因此常常需要写一些函数来简化cookie的功能。

以下函数摘自JavaScript高级程序设计。

```javascript
var CookieUtil = {

    get: function (name){
        var cookieName = encodeURIComponent(name) + "=",
            cookieStart = document.cookie.indexOf(cookieName),
            cookieValue = null,
            cookieEnd;
            
        if (cookieStart > -1){
            cookieEnd = document.cookie.indexOf(";", cookieStart);
            if (cookieEnd == -1){
                cookieEnd = document.cookie.length;
            }
            cookieValue = decodeURIComponent(document.cookie.substring(cookieStart + cookieName.length, cookieEnd));
        } 

        return cookieValue;
    },
    
    set: function (name, value, expires, path, domain, secure) {
        var cookieText = encodeURIComponent(name) + "=" + encodeURIComponent(value);
    
        if (expires instanceof Date) {
            cookieText += "; expires=" + expires.toGMTString();
        }
    
        if (path) {
            cookieText += "; path=" + path;
        }
    
        if (domain) {
            cookieText += "; domain=" + domain;
        }
    
        if (secure) {
            cookieText += "; secure";
        }
    
        document.cookie = cookieText;
    },
    
    unset: function (name, path, domain, secure){
        this.set(name, "", new Date(0), path, domain, secure);
    }

};
```

## Storage

### Storage对象的方法

clear(): 删除所有值，Firefox中没有实现。

getItem(name): 根据指定的name获取相应的值。

key(index): 获得index位置处的值得名字。

removeItem(name): 删除由name指定的名值对。

setItem(name, value): 为指定的name设置一个对应的值。

### sessionStorage 和 localStorage

sessionStorage 不受页面刷新影响，数据保存至标签页关闭。

localStorage 数据保留到通过JavaScript删除或者是用户清除浏览器缓存。 

对于localStorage而言，大多数桌面浏览器会设置每个来源（协议、域、端口）5MB的限制。Chrome 和Safari 对每个来源的限制是2. 5MB。而iOS 版Safari 和Android 版WebKit的限制也是2.5MB 。

对sessionStorage的限制也因浏览器而异。有的浏览器对sessionStorage的大小没有限制，
但Chrome 、Safari 、iOS 版Safari 和Android 版WebKit都有限制，也都是2.5MB 。IE8+和Opera 对
sessionStorage的限制是5MB 。

## IE userData

在IE5.0 中，微软通过一个自定义行为引人了持久化用户数据的概念。用户数据允许每个文档最多
128KB 数据，每个域名最多1MB 数据。

可以通过以下方式使用持久化数据。

1.首先需要使用CSS在元素上指定userDate行为。

```html
<div id="dataStore" style="behavior:url(#default#userData)"></div>
```

2.保存和获取数据。

```javascript
// 保存数据
dataStore.setAttribute("name", "Nicholas");
dataStore.setAttribute("book", "Professional JavaScript");
dataStore.save("BookInfo");

// 获取数据
dataStore.getAttribute("name");
```

## 参考目录

- `JavaScript高级程序设计（第三版）- 第23章 离线应用与客户端存储`