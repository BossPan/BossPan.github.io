---
layout:     post
title:      深入浅出Ajax / 学习笔记
summary:    Ajax 入门
categories: note
---

花了几个小时把书看完了，当时 Web 入门的书看的是那本 Head First HTML/CSS，所以对这系列的书存有好感， 但感觉这类风格的书不太适合现在的自己了...因为实在是太墨迹了...下面是笔记正文...

## XMLHttpRequest 对象

```
function createRequest() {
    var httpRequest = null;
    if (window.XMLHttpRequest) {
        httpRequest = new XMLHttpRequest();
    } else if (window.ActiveXObject) {
        try {
            httpRequest = new ActiveXObject('Msxml2.XMLHTTP');
        } catch (e) {
            try {
                httpRequest = new ActiveXObject('Microsoft.XMLHTTP')
            } catch (e) {
            }
        }
    }
    if (!httpRequest) {
        alert('Giving up :( Cannot create an XMLHTTP instance');
        return false;
    }
    return httpRequest;
}

function sendRequest() {
    var httpRequest = createRequest();
    httpRequest.onreadystatechange = stateChangeHandler;

    // GET
    // 直接在 url 后面添加参数信息 如 url?fname=Henry&lname=Ford
    // 由于浏览器缓存的存在，可以再添加一个 ID
    httpRequest.open(method, url, async);
    httpRequest.send();

    // POST
    // 必须指定 Content-type
    httpRequest.open(method, url, async);
    httpRequest.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    httpRequest.send(string);
}
```

## 重点回顾

- Ajax = Asynchronous JavaScript and XML
- 异步应用程序是使用 JavaScript 做出请求，而不是提交表单。
- 请求与响应是由浏览器处理，而不是直接由 JavaScript 代码处理。
- 一个请求对象可以作出多个请求，但请求对象一次只能追踪一个请求在服务器上的进展。
- 若要同时发出多个请求，则需要多个请求对象。
- readyState 是只读属性
- 浏览器不会缓存 POST 请求
- get 最大长度约 2000 字符
- POST 只比 GET 请求安全一点点，都需要额外的安全层如 SSL，才能保护数据的安全以免被人窥视。
- Content-Type 
	- x-www-form-urlenconded
	- text/xml 但几乎完全没有必要用 xml 送出请求
- SQL注入漏洞 mysql_real_escape_string()
- JSON JavaScript Object Notation
- 服务器端（如 PHP ） 如何解析JSON
- eval 将字符串转换为对象（JSON 文本 -> JSON 文本）
- escape()、unescape() => encodeURI()、decodeURI()






 





















