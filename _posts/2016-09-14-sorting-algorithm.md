---
title:      排序算法
summary:    常用排序算法 JS 实现
categories: JavaScript Algorithm
tags:       JavaScript Algorithm 算法
---

## 复杂度和稳定性

![排序算法的复杂度和稳定性](/img/sorting-algorithm.png)

## 插入排序

- 不适合对于数据量比较大的排序应用

```javascript
// 插入排序
function insertion_sort(arr) {
    // 方法一
    var temp;
    for (var i = 1; i < arr.length; i++) {
        temp = arr[i];
        for (var j = i - 1; j >= 0 && arr[j] > temp; j--) {
            arr[j + 1] = arr[j];
        }
        arr[j + 1] = temp;
    }

    // 方法二
    var temp;
    for (var i = 0; i < arr.length - 1; i++) {
        for (var j = i + 1; j > 0 && arr[j - 1] > arr[j]; j--) {
            temp = arr[j - 1];
            arr[j - 1] = arr[j];
            arr[j] = temp;
        }
    }
}
```

## 希尔排序

- 是对插入排序的改进

```javascript
// 希尔排序
function shell_sort(arr) {
    var len = arr.length;
    for(var step = len >> 1; step > 0; step >>= 1) {
        // 根据步长进行插入排序
        for(var i = step; i < len; i++) {
            var temp = arr[i];
            for(var j = i - step; j >= 0 && arr[j] > temp; j -= step) {
                arr[j + 1] = arr[j];
            }
            arr[j + step] = temp;
        }
    }
}
```

## 选择排序

- 选择排序是对冒泡排序的优化

```javascript
// 选择排序
function selection_sort(arr) {
    var min, temp;
    for (var i = 0; i < arr.length; i++) {
        min = i;
        for (var j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[min]) {
                min = j;
            }
        }
        temp = arr[min];
        arr[min] = arr[i];
        arr[i] = temp;
    }
}
```

## 堆排序

- [堆排序](http://www.cricode.com/977.html)

```javascript
// 堆排序
function heap_sort(arr) {
    function swap(i, j) {
        var temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    function max_heapify(start, end) {
        var parent = start,
            child = 2 * start + 1;
        if (child >= end) {
            return;
        }
        // 获取左右子节点中较大的值的下标，包括只有左子节点的情况
        if (child + 1 < end && arr[child] < arr[child + 1]) {
            child++;
        }
        if (arr[parent] < arr[child]) {
            swap(parent, child);
            max_heapify(child, end);
        }
    }

    var len = arr.length;
    // 从最后一个父节点开始，形成最大堆
    for (var i = parseInt(len / 2) - 1; i >= 0; i--) {
        max_heapify(i, len);
    }
    // 获取排序结果
    for (var i = len - 1; i > 0; i--) {
        swap(0, i);
        // 对除排序以外的值形成最大堆
        max_heapify(0, i);
    }
}
```

## 冒泡排序

```javascript
// 冒泡排序
function bubble_sort(arr) {
    // 方法一 大值后移
    var temp;
    for (var i = 0; i < arr.length - 1; i++) {
        for (var j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }

    // 方法二 小值前移
    var temp;
    for (var i = 0; i < arr.length; i++) {
        for (var j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[i]) {
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }

    // 优化->最好时间为O(n)
    var temp, swapState;
    for (var i = 0; i < arr.length - 1; i++) {
        swapState = false;
        for (var j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                // swapState
                swapState = true;
            }
        }
        if (swapState == false) {
            return;
        }
    }
}
```

## 快速排序

```javascript
// 快速排序
function quick_sort(arr) {
    if (arr.length < 2) {
        return arr.slice(0);
    }
    // 每次取第一个值为中间值(基准)
    var middle = [arr[0]],
        left = [],
        right = [];

    for (var i = 1; i < arr.length; i++) {
        if (arr[i] < middle[0]) {
            left.push(arr[i]);
        }
        else {
            right.push(arr[i]);
        }
    }
    return quick_sort(left).concat(middle, quick_sort(right));
}
```

## 归并排序

```javascript
// 归并排序
function merge_sort(arr) {
    var result = [];
    var merge = function (left, right) {
        while (left.length && right.length) {
            result.push(left[0] <= right[0] ? left.shift() : right.shift());
        }
        return result.concat(left.concat(right));
    }
    var len = arr.length;
    if (len < 2) {
        return arr;
    }
    var middle = parseInt(len / 2),
        left = arr.slice(0, middle),
        right = arr.slice(middle);
    return merge(merge_sort(left), merge_sort(right));
}
```

## 计数排序

- 只能用于正整数

```javascript
// 计数排序
function counting_sort(arr) {
    var len = arr.length,
        max = Math.max.apply(Math, arr);
    var count = new Array(max + 1),
        result = [];
    // 初始化count数组
    for (var i = 0; i <= max; i++) {
        count[i] = 0;
    }
    // 统计arr中等于count下标的个数
    for (var i = 0; i < len; i++) {
        count[arr[i]]++;
    }
    // 统计数组arr中值小于等于count数组下标的个数
    for (var i = 1; i < count.length; i++) {
        count[i] += count[i - 1];
    }
    // 排序结果
    for (var i = len - 1; i >= 0; i--) {
        result[count[arr[i]] - 1] = arr[i];
        // 由于arr可能出现相同值，因此需要更新count
        count[arr[i]]--;
    }
    return result;
}
```

## 参考目录

- [经典排序算法总结与实现](http://wuchong.me/blog/2014/02/09/algorithm-sort-summary/)
- [排序算法](https://zh.wikipedia.org/zh/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95)
- [面试中的 10 大排序算法总结](http://www.codeceo.com/article/10-sort-algorithm-interview.html)
- [常用排序算法稳定性、时间复杂度分析](http://www.cnblogs.com/nannanITeye/archive/2013/04/11/3013737.html)