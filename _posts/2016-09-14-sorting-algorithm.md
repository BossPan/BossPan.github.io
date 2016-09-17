---
title:      排序算法
summary:    常用排序算法JS实现
categories: JavaScript Algorithm
tags:       JavaScript Algorithm
---

## 冒泡排序

```javascript
// 冒泡排序
function bubble_sort(arr) {
	// 方法一 大值后移
	var temp;
	for(var i = 0; i < arr.length - 1; i++) {
		for(var j = 0; j < arr.length - 1 - i; j++) {
			if(arr[j] > arr[j + 1]) {
				temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		}
	}

	// 方法二 小值前移
	var temp;
	for(var i = 0; i < arr.length; i++) {
		for(var j = i + 1; j < arr.length; j++) {
			if(arr[j] < arr[i]) {
				temp = arr[i];
				arr[i] = arr[j];
				arr[j] = temp;
			}
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
	for(var i = 0; i < arr.length; i++) {
		min = i;
		for(var j = i + 1; j < arr.length; j++) {
			if(arr[j] < arr[min]) {
				min = j;
			}
		}
		temp = arr[min];
		arr[min] = arr[i];
		arr[i] = temp;
	}
}
```

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

## 归并排序

```javascript
// 归并排序
function merge_sort(arr) {
	var result = [];
	var merge = function(left, right) {
		while (left.length && right.length) {
			result.push(left[0] <= right[0] ? left.shift() : right.shift());
		}
		return result.concat(left.concat(right));
	}
	var len = arr.length;
	if(len < 2) {
		return arr;
	}
	var middle = parseInt(len / 2),
		left = arr.slice(0, middle),
		right = arr.slice(middle);
	return merge(merge_sort(left), merge_sort(right));
}
```

## 快速排序

```javascript
// 快速排序
function quick_sort(arr) {
	if(arr.length < 2) {
		return arr.slice(0);
	}
	var middle = [arr[0]],
		left = [],
		right = [];
	// 从基准之后的数据开始
	for(var i = 1; i < arr.length; i++) {
		if(arr[i] < middle[0]) {
			left.push(arr[i]);
		}
		else {
			right.push(arr[i]);
		}
	}
	return quick_sort(left).concat(middle.concat(quick_sort(right)));
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
		if(child >= end) {
			return;
		}
		// 获取左右子节点中较大的值的下标，包括只有左子节点的情况
		if(child + 1 < end && arr[child] < arr[child + 1]) {
			child++;
		}
		if(arr[parent] < arr[child]) {
			swap(parent, child);
			max_heapify(child, end);
		}
	}

	var len = arr.length;
	for(var i = parseInt(len / 2) - 1; i >=0; i--) {
		max_heapify(i, len);
	}

	for(var i = len -1; i > 0; i--) {
		swap(0, i);
		max_heapify(0, i);
	}
}
```

## 参考目录

- [经典排序算法总结与实现](http://wuchong.me/blog/2014/02/09/algorithm-sort-summary/)
- [排序算法](https://zh.wikipedia.org/zh/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95)
- [面试中的 10 大排序算法总结](http://www.codeceo.com/article/10-sort-algorithm-interview.html)