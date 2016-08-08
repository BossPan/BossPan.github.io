---
title:      元素的尺寸和坐标
summary:    width, innerWidth, clientWidth, offsetWidth, scrollWidth
categories: JavaScript
tags:       JavaScript 学习笔记
---

记得当时读 `JavaScript权威指南` 看到这么多相似的属性，我打心底拒绝的...

今天原本是要学习一下响应式网页设计的，因为最近项目应该要用到这个，后面从 viewport 查啊查，突然记得自己还有这么个坑没有填，所以就花时间整理了一下这块内容。

### 设备 pixels，css pixels

- 100% 缩放时，1设备pixels = 1 css pixels
- 缩小页面至 50%，1设备pixels容纳2个 css pixels
- 放大页面至 200%，1设备pixels容纳 0.5个 css pixels

### 获取元素的尺寸和位置

- getBoundingClientRect()
- 返回一个有left 、right 、top和bottom属性的对象。left和top属性表示元素的左上角的X和Y坐标， right和bottom属性表示元素的右下角的X和Y坐标。
- getBoundingClientRect() 返回的对象还包含width和height属性，但是在原始的IE中未实现。为了简便起见，可以这样计算元素的width和height。

```
var box = e.getBoundingClientRect();
var w = box.width || (box.right - box.left);
var h = box.height || (box.bottom - box.top);
```

### offsetWidth/Height

- 任何HTML元素的只读属性 offsetWidth 和 offsetHeight 以 css 像素返回它的屏幕尺寸。
- 包含元素的边框和内边距，不包含外边距。

### offsetLeft/Top

- 所有HTML元素拥有offsetLeft 和 offsetTop 属性来返回元素的 X 和 Y 坐标
- 文档坐标，井直接指定元素的位置。
- 对于己定位元素的后代元素和一些其他元素(如表格单元) ，这些属性返回的坐标是相对于**祖先元素**而非文档。
- 和 style.left/top 的区别
  - offsetTop 返回的是数字，而 style.top 返回的是字符串，并带有单位 px。
  - offsetTop 只读，而 style.top 可读写。
  - 如果没有给 HTML 元素指定过 top 样式，则 style.top 返回的是空字符串。
  - offsetWidth 与 style.width、offsetHeight 与 style.height 也是同样道理。

```
// 获取元素视口坐标的兼容性方案
function getElementPos(elt) {
    var x = 0,
        y = 0;
    //循环以累加偏移量
    for (var e = elt; e != null; e = e.offsetParent) {
        x += e.offsetLeft;
        y += e.offsetTop;
    }
    //再次循环所有的祖先元素，减去滚动的偏移量
    //这也减去了主滚动条，并转换为视口坐标
    for (var e = elt.parentNode; e != null && e.nodeType == 1; e = e.parentNode) {
        x -= e.scrollLeft;
        y -= e.scrollTop;
        return {
            x: x,
            y: y
        };
    }
}
```

### clientWidth/Height

- 类似 offsetWidth/Height . 不同的是它们不包含边框大小，只包含内容和它的内边距。
- 如果浏览器在内边距和边框之间添加了滚动条，clientWidth/Height 在其返回值中也不包含滚动条。
- 对于内联元素，总是返回0 。

### clientLeft/Top

- 元素的内边距的外边缘和它的边框的外边缘之间的水平距离和垂直距离，通常这些值就等于左边和上边的边框宽度。
- 如果元素有滚动条，并且浏览器将这些滚动条放置在左侧或顶部(可这不太常见) ，clientLeft/Top 也就包含了滚动条的宽度。
- 对于内联元素，总是为0。

### scrollWidth/Height

- 元素的内容区域加上它的内边距再加上任何**溢出内容的尺寸**。
- 当内容正好和内容区域匹配而没有溢出时，这些属性与 clientWidth/Height 是相等的。但当溢出时，它们就包含溢出的内容，返回值比 clientWidth/Height 要大。

### scrollLeft/Top

- 指定元素的滚动条的位置。
- 注意， scrollLeft 和 scrollTop 是可写的属性，通过设置它们来让元素中的内容滚动。
- HTML 元素并没有类似 Window 对象的 scrollTo 方法。

###  screen.width/height

- 屏幕宽度/高度
- 以设备pixels度量
- 兼容性：IE 以css pixels度量

### window.innerWidth/Height

- 浏览器窗口的宽度和高度（不包括工具栏、滚动条）
- 以css pixels 度量
- IE 8 及以下不支持

```
// 获取窗口的视口尺寸viewport的兼容性方案
// 标准 strict 模式
// 怪异模式的IE
var w = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;  

var h = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
```

### window.outerWidth/Height

- 浏览器窗口的宽度和高度（包括工具栏、滚动条）
- IE 8 及以下不支持

### window.pageX/YOffeset

- 页面(document)相对于窗口原点的水平、垂直位移，用于定位用户滚动了多少的滚动条距离
- 以css pixels 度量
- IE 8 及之前版本的 IE 不支持, 使用 document.body.scrollLeft /document.body.scrollTop 代替
- 原理上来说，在用户放大浏览器时，向上滚动了页面，window.pageX/YOffset 会改变。但当用户放大页面时，浏览器会尝试着保存用户当前可见的页面的元素依然在可见位置。虽然该特性表现得不如预期，但它意味着：在理论上该情况下  window.pageX/YOffset 并没有改变，被用户滚出屏幕的css pixels几乎保存不变。

```
// 获取滚动条位置的兼容性方案
function getScrollOffsets(w) {
    w = w || window;

    if (w.pageXOffset != null) return {
        x: w.pageXOffset,
        y: w.pageYOffset
    };

    var d = w.document;
    if (document.compatMode == "css1Compat")
        return {
            x: d.documentElement.scrollLeft,
            y: d.documentElement.scrollTop
        };

    return {
        x: d.body.scrollLeft,
        y: d.body.scrollTop
    };
}
```

### document.documentElement.offsetWidth/Height

- `<html>` 的尺寸
- IE 用这个值标示 viewport 的尺寸而非 `<html>`

### pageX/Y, clientX/Y, screenX/Y

- pageX/Y：从<html>原点到事件触发点的css pixels
- clientX/Y：从 viewport 原点（浏览器窗口）到事件触发点的 css pixels
- screenX/Y：从用户显示器窗口原点到事件触发点的设备 pixels
- IE不支持 pageX/Y,IE 使用 css pixels 来度量 screenX/Y

### 参考附录

- `JavaScript权威指南` 15.8 文档和元素的几何形状和滚动
- [A tale of two viewports — part one](http://www.quirksmode.org/mobile/viewports.html)
- [A tale of two viewports — part two](http://www.quirksmode.org/mobile/viewports2.html)
- [Window innerWidth and innerHeight Properties](http://www.w3schools.com/jsref/prop_win_innerheight.asp)
- [clientWidth offsetWidth innerWidth 区别（窗口尺寸汇总）](http://www.cnblogs.com/youxin/archive/2012/09/21/2697514.html)