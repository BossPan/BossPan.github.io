---
layout:     post
title:      深入浅出Ajax / 学习笔记
summary:    Ajax 入门
categories: note
---

花了几个小时把书看完了，当时 Web 入门的书看的是那本 Head First HTML/CSS，所以对这系列的书存有好感， 但感觉这类风格的书不太适合现在的自己了...因为实在是太墨迹了...下面是笔记正文...

## [XMLHttpRequest 对象](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

### 创建请求对象 / 发送请求
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
            if (!httpRequest) {
                throw new Error('HTTP Request Not Supported');
            }
        }
        return httpRequest;
    }

    function sendRequest() {
        var httpRequest = createRequest();
        httpRequest.onreadystatechange = callback;

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

    function callback() {
        if (httpRequest.readyState == 4 && httpRequest.status == 200) {
            // do something
        }
    }
```

### XMLHttpRequest 重要属性 / 方法
- onreadystatechange
- readystate
- status
- timeout
- responseText
- responseXML
- setRequestHeader

## SQL 注入漏洞
- 前台 JavaScript： 发送之前确认
- 服务器端（以 PHP 为例）：使用 mysql_real_escape_string() 处理接受到的表单项

## [JSON](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- JavaScript Object Notation
- JSON 与 Javascript 的区别
	- 对象/数组：属性名称必须用双引号包裹；最后一个属性后面不能有逗号。
	- 数值：前导0不能使用；小数点后面至少有一个数字。
	- 字符串：只有有限的字符能够被转义;通常不允许控制字符; 但允许使用Unicode 行分隔符 (U+2028) 和段落分隔符 (U+2029) ; 字符串必须用双引号括起来。
- JSON.parse(text[, reviver])
	- 将一个 JSON 字符串解析成为一个 JavaScript 值。在解析过程中，还可以选择性的修改某些属性的原始解析值。
- JSON.stringigy(value[, replacer [, space]])
	- 可以将任意的 JavaScript 值序列化成 JSON 字符串。

## 重点回顾

- Ajax = Asynchronous JavaScript and XML
- 异步应用程序是使用 JavaScript 做出请求，而不是提交表单。
- 请求与响应是由浏览器处理，而不是直接由 JavaScript 代码处理。
- 一个请求对象可以作出多个请求，但请求对象一次只能追踪一个请求在服务器上的进展。
- 若要同时发出多个请求，则需要多个请求对象。
- readyState 是只读属性
- 浏览器不会缓存 POST 请求
- get 最大长度 2KB
- POST 只比 GET 请求安全一点点，都需要额外的安全层如 SSL，才能保护数据的安全以免被人窥视。
- Content-Type 
	- x-www-form-urlenconded （名值对）
	- text/xml 但几乎完全没有必要用 xml 送出请求
- 了解服务器端（如 PHP ） 如何解析 JSON
- eval 将字符串转换为对象（JSON 文本 -> JSON 对象）-> JSON.stringify
- escape()、unescape() -> encodeURI()、decodeURI()






 





















