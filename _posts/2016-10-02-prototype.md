---
title:      理解 JavaScript 的原型机制
summary:    prototype, constructor, instanceof
categories: JavaScript
tags:       JavaScript
---

## 原型对象

每个函数都有一个prototype（原型）属性，这个属性指向函数的原型对象。

默认情况下，所有原型对象会自动获得一个constructor（构造函数）属性，这个属性包含一个指向prototype属性所在函数的指针。

每个实例包含一个内部属性[[prototype]]，指向构造函数的原型对象。虽然在脚本中没有标准的方式访问[[prototype]]，但Firefox、Safari、Chrome将其实现为`__proto__`。

## 原型链

由于每个原型对象也是一个对象实例，因此也会有[[prototype]]属性，这样就可以通过[[prototype]]层层递进，形成实例与原型的链条，这就是原型链的概念。

```javascript
function SuperType(){
    this.property = true;
}

SuperType.prototype.getSuperValue = function(){
    return this.property;
};

function SubType(){
    this.subproperty = false;
}

// 继承SuperType
SubType.prototype = new SuperType();
// 显式设置constructor
SubType.prototype.constructor = SubType;

SubType.prototype.getSubValue = function (){
    return this.subproperty;
};

var instance = new SubType();
console.log(instance.getSuperValue());   //true
```

通过把SubType的原型对象设置为SuperType的实例，实现了继承。

原型链如下图所示。

![原型链示意图](/img/prototype-chain-example.png)

原型链在浏览器中的实现。

![__proto__](/img/prototype-chrome.png)

## 为什么继承时需要显式设置SubType.prototype.constructor

重写子类的原型对象时，默认的原型对象constructor属性被删除，此时获取实例的constructor属性，通过原型链查找，可以在SuperType.prototype中找到，值为SuperType。这和我们期望的SubType不符，因此需要显式设置其值。

## 通过new操作符调用构造函数的实际过程是什么

1.创建一个新对象

```
var obj = {}; 
```

2.实现这个新对象的一个内部属性[[prototype]]，Firefox、Safari、Chrome实现为__proto__。假设构造函数为Constructor。

```javascript
obj.__proto__ = Constructor.prototype;
```



3.在obj上执行构造函数中的代码（为这个新对象添加属性）

```javascript
Constructor.call(obj);
```

4.返回这个新对象



## 如何确定一个实例的类型

object instanceof Constructor，检测 `Constructor.prototype `是否存在于对象的原型链上。

或者使用isPrototypeOf()，如下例所示。

```javascript
console.log(instance instanceof Object);      //true
console.log(instance instanceof SuperType);   //true
console.log(instance instanceof SubType);     //true

console.log(Object.prototype.isPrototypeOf(instance));    //true
console.log(SuperType.prototype.isPrototypeOf(instance)); //true
console.log(SubType.prototype.isPrototypeOf(instance));   //true
```

## 参考目录

- `JavaScript高级程序设计（第三版）- 第六章 面向对象的程序设计`