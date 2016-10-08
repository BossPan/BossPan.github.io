---
title:      等高瀑布流布局
summary:    等高瀑布流布局的实现
categories: CSS Layout
tags:       CSS JavaScript 网页布局
---

##  关键点

- 将图片分行展示
- 确定每一行的高度

## 实现

### 数据加载

```javascript
var count = 0;
for (i = 0; i < data.list.length; i++) {
    var img = document.createElement('img');
    img.src = 'images/' + data.list[i].src;
    img.onload = function () {
        count++;
        // 在所有图片请求完成后才调用瀑布流函数，否则在函数中获取的图片宽度高度为0
        if (count === data.list.length) {
            waterfall();
        }
    };
    container.appendChild(img);
}
```

### 根据浏览器宽度，制定出标准高度

```javascript
// 根据浏览器宽度，制定出标准高度referHeight
var sumWidth = document.documentElement.clientWidth || document.body.clientWidth;
var referHeight;
if (sumWidth >= 1920) {
    referHeight = 250;
} else if (sumWidth >= 960) {
    referHeight = 210;
} else {
    referHeight = 170;
}
```

### 将所有图片等比变化到该高度

```javascript
// 对所有图片基于referHeight进行等比例缩放
var imgLists = container.getElementsByTagName('img');
for (var i = 0; i < imgLists.length; i++) {
    imgLists[i].height = referHeight;
}
```

### 每行图片包上一个div容器

```javascript
// 每行建立一个div，并装入尽可能多的图片，直到容器装不下
var curWidth = 0;
var div = document.createElement('div');
for (i = 0, len = imgLists.length; i < len; i++) {
    curWidth += imgLists[0].width;
    if (curWidth > sumWidth) {
        container.appendChild(div);
        curWidth = imgLists[0].width;
        div = document.createElement('div');
    }
    div.appendChild(imgLists[0]);
}
// 最后一个div
container.appendChild(div);
```

### 缩放每行至同一宽度，调整误差

```javascript
var rows = container.getElementsByTagName('div'),
    scale, // 缩放比
    imgs,
    rowWidth;

for (i = 0, len = rows.length; i < len; i++) {
    imgs = rows[i].getElementsByTagName('img');

    rowWidth = 0;
    for (var j = 0; j < imgs.length; j++) {
        rowWidth += imgs[j].width;
    }

    scale = sumWidth / rowWidth;
    if(scale > 1.5 ) {
        return;
    }
    // 缩放到和浏览器同宽
    for (j = 0; j < imgs.length; j++) {
        imgs[j].height *= scale;
    }

    // 调整误差值
    // code here
}
```

## 参考目录

- [网页上的摄影展:等高响应布局实现](http://isux.tencent.com/high-equal-response-layout-html.html)
- [非等宽图片列表的布局](http://stylechen.com/not-fixed-width-imglist-layout.html)
- [再谈等高瀑布流布局的算法](http://stylechen.com/fixed-height-waterfall.html)