---
title:      二分查找算法
summary:    二分查找算法的非递归和递归实现
categories: JavaScript Algorithm
tags:       JavaScript Algorithm 算法
---

二分查找要求待查表为有序表，时间复杂度为O(logn)。

假设元素按升序排列，则JavaScript实现如下。

## 非递归方式

```javascript
/**
 * 非递归方式
 * @param arr
 * @param num
 * @returns {number}
 */
function binarySearch(arr, num) {
    var startIndex = 0,
        endIndex = arr.length - 1,
        len = endIndex - startIndex + 1;
    if (!len) {
        return -1;
    }
    var n = startIndex + Math.floor(len / 2);
    while (len >= 1) {
        if (arr[n] === num) {
            return n;
        } else if (arr[n] > num) {
            endIndex = n - 1;
        } else {
            startIndex = n + 1;
        }
        len = endIndex - startIndex + 1;
        n = startIndex + Math.floor(len / 2);
    }
    return -1;
}
```



## 递归方式

```javascript
/**
 * 递归方式
 * @param arr
 * @param num
 * @param startIndex
 * @param endIndex
 * @returns {number}
 */
function recursiveBinarySearch(arr, num, startIndex, endIndex) {
    if (endIndex === undefined) {
        startIndex = 0;
    }
    if (endIndex === undefined) {
        endIndex = arr.length - 1;
    }
    var len = endIndex - startIndex + 1;
    if (!len) {
        return -1;
    }
    var mid = startIndex + Math.floor(len / 2);
    if (arr[mid] === num) {
        return mid;
    } else if (arr[mid] > num) {
        return arguments.callee(arr, num, startIndex, mid - 1);
    } else {
        return arguments.callee(arr, num, mid + 1, endIndex);
    }
}
```

