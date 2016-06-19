---
layout:     post
title:      JavaScript 对象继承
summary:    对象继承的方法总结
categories: note
---

## 一 构造函数的继承

### 1. 对象冒充 / 构造函数绑定

- 可以实现多重继承
- 对于同名属性或方法，后继承的类具有高优先级

```
function Class() {
    // 只有超类中的参数顺序与子类中的参数顺序完全一致时才可以传递参数对象
    // 如果不是，就必须创建一个单独的数组，按照正确的顺序放置参数
    SubClass.apply(this, arguments);
}
```

### 2. 原型链继承

- 不支持多重继承

#### 2.1 子类的 prototype 属性指向父类的实例
```
Class.prototype = new SubClass();
// 避免继承链的紊乱
Class.prototype.constructor = Class;
```

```
function Animal() {
    this.species = '动物';
}

function Cat(name, color) {
    this.name = name;
    this.color = color;
}

var cat = new Cat("大毛", "黄色");
cat.prototype = new Animal();
cat.prototype.constructor = Cat;
console.log(cat.prototype.species); // 动物
```

#### 2.2 直接继承父类的 prototype
- 注意父类必须显示实现 prototype

```
Class.prototype = SubClass.prototype;
Class.prototype.constructor = Class;
```

```
function Animal(){ }
Animal.prototype.species = "动物";

function Cat(name, color) {
    this.name = name;
    this.color = color;
}

Cat.prototype=Animal.prototype;
var cat = new Cat("大毛","黄色");
console.log(cat.species); // 动物
```

#### 2.3 上一方法的改进

```
function extend(Child, Parent) {
    var F = function () {};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
    Child.uber = Parent.prototype;
}
```

```
// Animal 和 Cat 的定义同 2.2
extend(Cat, Animal);
var cat = new Cat("大毛", "黄色");
console.log(cat.species); // 动物
```

### 3. 混合方式
- 使用对象冒充继承构造函数内的属性
- 用原型链继承 prototype 对象的方法

```
function ClassA(sColor) {
    this.color = sColor;
}

ClassA.prototype.sayColor = function () {
    alert(this.color);
};

function ClassB(sColor, sName) {
    ClassA.call(this, sColor);
    this.name = sName;
}

ClassB.prototype = new ClassA();

ClassB.prototype.sayName = function () {
    alert(this.name);
};
```

### 4. 拷贝继承
```
function extend(Class, SubClass) {
    var s = SubClass.prototype;
    var c = SubClass.prototype;
    for (var i in s) {
        c[i] = s[i];
    }
    c.uber = s;
}
```

## 二 非构造函数继承

### 1. object 方法

```
function object(o) {
        function F() {}
        F.prototype = o;
        return new F();
    }
```

```
var Chinese = {
        nation: '中国'
};
var Doctor = object(Chinese);
console.log(Doctor.nation); //中国
```

### 2. 浅拷贝

```
function shallowCopy(parent) {
    var copy = {};
    for (var i in parent) {
        copy[i] = parent[i];
    }
    copy.uber = parent;
    return copy;
}
```

### 3. 深拷贝([参照 JavaScript 对象复制](http://bosspan.github.io/note/2016/06/19/object-clone/))

```
function deepCopy(parent, child) {
    var copy = child || {};
    for (var i in parent) {
        if (parent.hasOwnProperty(i)) {
            if (typeof parent[i] == 'object') {
                parent[i].constructor == Array ? [] : {};
                deepCopy(parent[i], copy[i]);
            }
            else {
                copy[i] = parent[i];
            }
        }
    }
    return copy;
}
```

## 参考目录

- [Javascript面向对象编程（二）：构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)
- [ECMAScript 继承机制实现](http://www.w3school.com.cn/js/pro_js_inheritance_implementing.asp)



 





















