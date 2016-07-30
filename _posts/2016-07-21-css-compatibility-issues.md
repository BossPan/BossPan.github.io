---
title:      CSS 兼容性问题
summary:    CSS 常见兼容性问题及解决方案
categories: CSS
tags:       Note CSS
---

浏览器的差异性带来的问题不容忽视，所以打算借这篇文章对CSS的兼容性问题做一个汇总。在开始之前先推荐一个网站 [Can I use](http://caniuse.com/)，一个查询各属性在浏览器的兼容性的网站。

## 浏览器默认样式

首先是浏览器之间的默认样式各不相同，尤为明显的应该是表单控件的样式，比如input的padding,border等等。

### 解决方案

在reset.css中做设置，清除默认样式。可以参考 [CSS Tools: Reset CSS](http://meyerweb.com/eric/tools/css/reset/)，或者YUI的 [CSS Reset](http://yuilibrary.com/yui/docs/cssreset/)。 

## IE6 外边距加倍

IE6下浮动元素在浮动方向上与父元素边界接触元素的外边距会加倍。

### 解决方案 

1.使用padding控制间距。 

2.浮动元素display: inline。这样解决问题且无任何副作用：css标准规定浮动元素display:inline会自动调整为block。

## 属性的支持情况(IE)

| 属性                                       | IE6  | IE7  | IE8  | IE9  | IE10 |
| ---------------------------------------- | ---- | ---- | ---- | ---- | ---- |
| [min/max-width/height](http://caniuse.com/#search=min-height) |      | √    |      |      |      |
| media query                              |      |      |      | √    |      |
| first-child                              |      | √    |      |      |      |
| box-sizing                               |      |      | √    |      |      |
| border-radius                            |      |      |      | √    |      |
| autofocus                                |      |      |      |      | √    |
| placeholder                              |      |      |      |      | √    |

## 背景透明文字不透明

rgba IE9+,opacity IE9+

IE8 以及更早的版本支持替代的 filter 属性。例如：filter:Alpha(opacity=50)。

### 解决方案

```scss
// 方法1，增加蒙版DIV，为蒙版设置透明度
.mask {
  background-color: #000;
  filter:alpha(opacity=20);
  opacity: 20;
}

// 方法2 
.login {
  background-color: rgba(0, 0, 0, 0.2); // IE9+
}
@media \0screen\,screen\9 { // IE6,7,8
  .login {
    background: #000;
    filter: Alpha(opacity=20);
    *zoom: 1; // 激活IE6、7的haslayout属性，让它读懂Alpha 
  }
  .login h2, .login form {
    position: relative; position: relative; // 设置子元素为相对定位，可让子元素不继承Alpha值
  }
}
```

## 参考目录

- [CSS实现背景透明，文字不透明，兼容所有浏览器](http://www.cnblogs.com/PeunZhang/p/4089894.html)