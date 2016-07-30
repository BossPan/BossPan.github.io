---
title:      JavaScript 数组去重
summary:    了解数组去重
categories: JavaScript
tags:       Note JavaScript 学习笔记
---

考虑数组去重这个问题，JavaScript 里最容易想到的方法应该是下面这个

```
    function unique(arr) {
        var newArr = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            if (arr.indexOf(arr[i]) == -1) {
                newArr.push(arr[i]);
            }
        }
        return newArr;
    }
```

但是 indexOf 方法 IE8 及以下不支持，所以需要对 indexOf 改进一下。像下面这样

```
    // 兼容 IE 的 indexOf 函数
	var indexOf = [].indexOf ? function (arr, item) {
        return arr.indexOf(item);
    } : function (arr, item) {
        for (var i = 0, len = item.length; i < len; i++) {
            if (arr[i] === item) {
                return i;
            }
        }
        return -1;
    };

	function unique(arr) {
        var newArr = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            if (indexOf(newArr, arr[i]) == -1) {
                newArr.push(arr[i]);
            }
        }
        return newArr;
    }
```
有一种类似的方法，同样是用到了两重循环，只是循环的思路不太一样，并且没有用到 indexOf 函数。

```
    function unique(arr) {
        var newArr = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            for (var j = i + 1; j < len; j++) {
                // 如果有相同项则跳过
                if (arr[i] === arr[j]) {
                    j = ++i;
                }
            }
            // 最后没有相同项，push
            newArr.push(arr[i]);
        }
        return newArr;
    }
```
如果数组很大，上面两种方法就效率很低。一种改进方法是使用 Hash 对象。

```
    function unique(arr) {
        var newArr = [];
        var hash = {};
        for (var i = 0, len = arr.length; i < len; i++) {
            if (!hash[arr[i]]) {
                hash[arr[i]] = true;
                newArr.push(arr[i]);
            }
        }
        return newArr;
    }
```
借助于 Hash 对象的问题在于，值 1 和 '1' 被认为是相同的，改进一下。

```
    function unique(arr) {
        var newArr = [];
        var hash = {};
        for (var i = 0, len = arr.length; i < len; i++) {
            var key = typeof arr[i] + arr[i];
            if (!hash[key]) {
                hash[key] = true;
                newArr.push(arr[i]);
            }
        }
        return newArr;
    }
```

参考目录

- [javascript数组 去重](http://www.cnblogs.com/duanhuajian/p/3499993.html)
- [从 JavaScript 数组去重谈性能优化](http://blog.jobbole.com/33099/)








 





















