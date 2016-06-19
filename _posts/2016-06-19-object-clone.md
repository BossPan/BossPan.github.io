---
layout:     post
title:      JavaScript 对象复制
summary:    对象的深复制和浅复制
categories: note
---

## 浅复制
- 只复制一份引用/结构，所有引用对象都指向一份数据，并且都可以修改这份数据。

## 深度复制
- 为结构和数据创建副本，并指向这个副本，此后的操作都在副本上进行，不会影响到复制源。
- 基本包装类型（Number、Boolean、String）可以通过普通赋值方式实现。
- 需要注意的是，重写某个引用类型，只是将该变量指针指向别处。
- 下面是包装的复制函数

```
// clone
function clone(obj) {
	// 对于 Function
	if (obj instanceof Function) {
        var copy = function () {
            return obj.apply(this, arguments);
        };
        for (var prop in obj) {
            if (obj.hasOwnProperty(prop)) {
                copy[prop] = clone(obj[prop]);
            }
        }
        console.log(aaa);
        return copy;
    }
	// null, undefined, String, Boolean, Number
    if (obj == null || typeof obj !== "object") {
        return obj;
    }
    if (obj instanceof Date) {
        var copy = new Date();
        copy.setTime(obj.getTime());
        return copy;
    }
    if (obj instanceof Array) {
        var copy = [];
        for (var i = 0; i < obj.length; i++) {
            copy[i] = clone(obj[i]);
        }
        return copy;
    }
    if (obj instanceof Object) {
        var copy = {};
        for (var prop in obj) {
            if (obj.hasOwnProperty(prop)) {
                copy[prop] = clone(obj[prop]);
            }
        }
        return copy;
    }
    throw new Error("Unable to copy " + obj + " ! Its type isn't supported.");
}
```

或者将 Function 的深复制独立出来。

```
Function.prototype.copy = function() {
    var that = this;
    var temp = function() { 
    	return that.apply(this, arguments); 
    };
    for(var key in this) {
        if (this.hasOwnProperty(key)) {
            temp[key] = this[key];
        }
    }
    return temp;
};
```


 





















