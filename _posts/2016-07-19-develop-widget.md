---
title:      JavaScript 插件开发
summary:    JavaScript 插件开发，jQuery 插件开发
categories: JavaScript
tags:       JavaScript 学习笔记
---

最近要为页面写一个分页的功能，在网上看了几个 jQuery 分页插件的效果，发现跟项目的需求不太相符，权衡了一下，还是决定自己写。虽然不如高手写的那么完美，但至少更轻量级一些，更适合自己手里的这个项目，还能锻炼自己的能力。

## 从 JavaScript 插件说起

需要注意的几个点：

1.自调用匿名函数包裹，避免全局作用域污染。

```
(function(){})();
```

或者

```
(function(){}());
```

关于自调用函数的详解及以上两者的区别，可以阅读 [立即调用的函数表达式](http://www.cnblogs.com/TomXu/archive/2011/12/31/2289423.html) 。

2.在文件开头添加 ";"，避免代码合并等错误。

```javascript
;
(function(window) {
	var app = {};
	// ...
	window.plugin = app;
})(window);
```

## 关于 jQuery 插件开发

在插件中使用$别名

```
(function($) {
	
})(jQuery);
```

添加新的全局函数

```javascript
(function($) {
	$.sum = function(array) {
		//在这里添加代码
	};
})(jQuery);

// 调用方法
$.sum();
```

添加多个全局函数

1.扩展全局jQuery对象

```javascript
(function($) {
	$.extend({
		sum: function(array) {

		},
		average: function(array) {

		}
	});
})(jQuery);
```

2.使用命名空间隔离函数

```javascript
(function($) {
	$.mathUtils = {
		sum: function(array) {
			
		},
		average: function(array) {
			
		}
	};
})(jQuery);
```

添加jQuery对象方法

```javascript
jQuery.fn.myMethod = function() {
	
};
// jQuery.fn 对象是jQuery.prototype的别名
```

1.对象方法的上下文

在任何插件方法内部，关键字this引用的都是当前的jQuery对象。

2.隐式迭代

```javascript
(function($) {
	$.fn.swapClass = function() {
		this.each(function() {
			var $element = $(this);
			//
		});
	};
})(jQuery);
```

3.方法连缀

在所有插件方法中返回一个jQuery对象，除非相应的方法明显用于取得不同的信息

```javascript
(function($) {
	$.fn.swapClass = function() {
		return this.each(function() {
			var $element = $(this);
			//
		});
	};
})(jQuery);
```

4.提供灵活的参数

默认参数值

```javascript
var defaults = {
	copies: 5,
	opacity: 0.1
};
var options = $.extend(defaults, opts);
```

可定制的默认值

```javascript
var options = $.extend({}, $.fn.shadow.defaults, opts);
// 默认值被放在了投影插件的命名空间里，可以通过$.fn.shadow.defaults直接引用。
$.fn.shadow.defaults = {
	copies: 5,
	opacity: 0.1,
	copyOffset: function(index) {
		return {
			x: index,
			y: index
		};
	}
};
```

## 分页插件编写实践

```
;
(function() {
	var options = {
		numPerPage: 10,
		curPage: 0,
		maxPage: 0,
		minPage: 0,
		sortDirection: 1
	};
	var api = {
		config: options,
		init: function(opt) {
			for (var key in opt) {
				options[key] = opt[key];
			}
			options.maxPage = Math.floor(opt.data.length / options.numPerPage);

			var numSpan = document.getElementById('numPerPage');
			var maxPage = document.getElementById('maxPage');
			numSpan.innerHTML = options.numPerPage;
			maxPage.innerHTML = options.maxPage + 1;

			var paginationDiv = document.getElementById('page-nav');
			EventUtil.addHandler(paginationDiv, 'click', pageHandler);

			options.render(options.curPage);
		}
	};

	function pageHandler(event) {
		var target = EventUtil.getTarget(event);
		var id = target.getAttribute('ID');
		switch (id) {
			case 'pre':
				if (options.curPage == options.minPage) return;
				options.curPage--;
				break;
			case 'next':
				if (options.curPage == options.maxPage) return;
				options.curPage++;
				break;
			case 'first':
				options.curPage = options.minPage;
				break;
			case 'last':
				options.curPage = options.maxPage;
				break;
			case 'page-to':
				var page = document.getElementById('page-input').value - 1;
				if (page >= options.minPage && page <= options.maxPage) {
					options.curPage = page;
				}
				break;
		}
		options.render(options.curPage);
	}
	this.pagination = api;
})();
```

调用方法

```javascript
pagination.init({
	data: userData,
	render: renderList
});
```

## 参考目录

- `jQuery基础教程 第四版`
- [如何自己开发一款js或者jquery插件](http://www.haorooms.com/post/js_jquery_chajian)
- [Javascript插件开发的一些感想和心得](http://luopq.com/2016/02/04/think-js-plugin/)