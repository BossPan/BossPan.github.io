---
layout:     post
title:      网站导航栏共用
summary:    如何在一个 html 文件中引用另一个 html 文件
categories: note
---


## JavaScript

- 把 html 的内容在 js 文件中 使用 `document.write` 输出
- 在需要的地方通过 `<script>` 标签引入

```
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

## PHP / ASP

- include 引入文件

```
    <?php include 'header.php';?>
```

## Ajax / jQuery

- Ajax 加载内容

```
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

```
    <script>
	  $(document).ready(function () {
	  	$('body').load('header.html');
	  });
    </script>
```

## 参考目录

- [Include another HTML file in a HTML file](http://stackoverflow.com/questions/8988855/include-another-html-file-in-a-html-file)
- [How to Include an HTML File in Another](http://webdesign.about.com/od/ssi/a/aa052002a.htm)