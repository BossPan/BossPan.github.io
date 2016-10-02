---
title:      网站导航栏
summary:    如何实现导航栏复用
categories: HTML
tags:       HTML 学习笔记
---

## JavaScript

- 把 html 的内容在 js 文件中 使用 `document.write` 输出
- 在需要的地方通过 `<script>` 标签引入

```javascript
// header.js
document.write(`
    <div class="header">
        <ul class="nav">
            <li>A</li>
            <li>B</li>
            <li>C</li>
            <li>D</li>
        </ul>
    </div>
`);
```

## Ajax

- Ajax 加载内容

```javascript
// getHeader.js
function sendRequest(method, url, data) {
	// ...
  function callback() {
    if(httpRequest.readyState == 4 && httpRequest.status == 200) {
	  document.body.innerHTML = httpRequest.responseText;
    }
  }
}
sendRequest('get', '../html/header.html', null);

// 页面引用
<script src="../js/getHeader.js"></script>
```
- $().load();

```javascript
<script>
  $(document).ready(function () {
  	$('body').load('header.html');
  });
</script>
```

## PHP

- include 引入文件

```php
<?php include 'header.php';?>
```

## 参考目录

- [Include another HTML file in a HTML file](http://stackoverflow.com/questions/8988855/include-another-html-file-in-a-html-file)
- [How to Include an HTML File in Another](http://webdesign.about.com/od/ssi/a/aa052002a.htm)