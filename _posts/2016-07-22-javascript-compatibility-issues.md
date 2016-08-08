---
title:      JavaScript 兼容性问题
summary:    JavaScript 常见浏览器兼容性问题及解决方案
categories: JavaScript
tags:       JavaScript 兼容性
---

## 事件机制

```javascript
element.addEventListener(type, handler, false);
```

- 默认值为 false，处理函数在冒泡阶段被调用，如果传递了 true，则注册为捕获事件。
- IE 8 及之前版本不支持事件捕获，IE5及以后版本定义了 attachEvent() 和detachEvent() 。
- 利用attachEvent 注册的处理函数调用时this指向不再是先前注册事件的元素，这时的this为window对象。

## innerHTML

- IE9及以前  col, colgroup, frameset, html, head, style, table, tbody, tfoot, thead, title, tr 的 innerHTML, insertAdjacentHTML 属性为只读属性。
- In IE10 and IE11, when using innerText, innerHTML or outerHTML on a textarea which has a placeholder attribute, the returned HTML/text also uses the placeholder's value as the actual textarea's value. See bug.


### 解决方法

借助于 div 的 innerHTML 属性。

## 常用函数(属性)的兼容性

|           属性           | IE6  | IE7  | IE8  | IE9  | IE10 |
| :--------------------: | :--: | :--: | :--: | :--: | :--: |
|      textContent       |      |      |  √   |      |      |
|          trim          |      |      |      | √(P) | √(S) |
| getElementsByClassName |      |      |      |  √   |      |
|      getAttribute      |  √   |      |      |      |      |
|          bind          |      |      |      | √(P) | √(S) |
|    DocumentFragment    |      |      |      |  √   |      |

## 注意

- table tbody tr td 之间的依赖关系
- innerText firefox45+ 支持
- IE11 不支持 ``
