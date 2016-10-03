---
title:      等宽瀑布流布局
summary:    等宽瀑布流布局的实现
categories: CSS Layout
tags:       CSS JavaScript 网页布局
---

##  关键点

- 计算显示的列数
- 对每一个数据块进行定位
- 判断数据加载的条件

## 实现

### HTML结构

```html
<div class="waterfall">
    <div class="item-box">
        <div class="img-box">
            <img src="images/0.jpg" alt="">
        </div>
    </div>
    <div class="item-box">
        <div class="img-box">
            <img src="images/1.jpg" alt="">
        </div>
    </div>
    <div class="item-box">
        <div class="img-box">
            <img src="images/2.jpg" alt="">
        </div>
    </div>
    <!-- ... -->
</div>
```

### CSS 样式

```css
html, body {
    margin: 0;
    padding: 0;
}

.waterfall {
    position: relative;
    margin: 0 auto;
}

.item-box {
    padding: 10px;
    float: left;
}

.img-box {
    padding: 10px;
    border:1px solid #ccc;
    box-shadow: 0 0 6px #ccc;
    border-radius: 5px;
}

.img-box img {
    width: 165px;
    height: auto;
}

```

### 图片定位

方法1 通过 JavaScript 对图片进行定位

- 需要实现知道数据块高度，如果其中包含图片，需要知道图片高度。
- JS 动态计算数据块位置，当窗口缩放频繁，可能会狂耗性能。

```javascript
function repostion() {
    var container = document.getElementsByClassName('waterfall')[0],
        items = document.getElementsByClassName('item-box'),
        itemWidth = items[0].offsetWidth,
        sumWidth = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;

    // 计算列数
    var colNum = Math.floor(sumWidth / itemWidth);

    // 设置container的宽度实现居中
    container.style.width = colNum * items[0].offsetWidth + 'px';

    var heightArr = [],
        minHeight, minIndex;
    for (var i = 0, len = items.length; i < len; i++) {
        if (i < colNum) {
            heightArr.push(items[i].offsetHeight);
        }
        else {
            // 计算最短列
            minHeight = Math.min.apply(null, heightArr);
            minIndex = heightArr.indexOf(minHeight);
            // 对图片进行定位
            items[i].style.position = 'absolute';
            items[i].style.left = minIndex * itemWidth + 'px'; // items[minIndex].offsetLeft
            items[i].style.top = heightArr[minIndex] + 'px';
            // 更新数组
            heightArr[minIndex] += items[i].offsetHeight;
        }
    }
}
```

方法2 使用 CSS3 的`column`属性

- 不需要计算，性能高
- 只有高级浏览器中才能使用。图片排序按照垂直顺序排列，打乱图片显示顺序
- 列间距随浏览器窗口改变，用户体验不好

```css
.waterfall {
    -moz-column-width: 207px;
    -webkit-column-width: 207px;
    column-width: 207px;
    -moz-column-gap: 5px;
    -webkit-column-gap: 5px;
    column-gap: 5px;
}
```

## 数据加载

```javascript
window.onscroll = function () {
    var container = document.getElementsByClassName('waterfall')[0];
    // 满足加载条件时加载数据
    if (checkScroll()) {
        // 更新DOM
        // ...
    }
    // 对所有数据块进行重新定位
    repostion();
}

// 判断是否满足数据加载的条件
function checkScroll() {
    var items = document.getElementsByClassName('item-box'),
        last = items[items.length - 1];
    var lastH = last.offsetTop + Math.floor(last.offsetHeight / 2),
        scrollTop = document.documentElement.scrollTop || document.body.scrollTop,
        documentH = window.innerHeight || document.documentElement.offsetHeight || document.body.clientHeight;
    // 当最后一张图片显示一半时，加载数据
    return lastH < scrollTop + documentH;
}
```

## 参考目录

- [瀑布流布局浅析](http://www.68design.net/Web-Guide/HTMLCSS/58734-1.html)
- [瀑布流布局 - 慕课网](http://www.imooc.com/learn/101)