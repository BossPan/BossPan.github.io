---
layout:     post
title:      JavaScript 对象创建与继承
summary:    对象创建、继承方法总结
categories: note
---

## 一 对象创建 
- 对象字面量
- 构造函数
- JSON

调用构造函数 new 的过程

```
var Person = function(name) {
    this.name = name;
}
var p = new Person();

// new 操作符的操作是
var p = {}
p.__proto__ =  Person.prototype
Person.call(p);
``` 

## 二 构造函数的继承

### 1. 对象冒充 / 构造函数绑定

- 可以实现多重继承
- 对于同名属性或方法，后继承的类具有高优先级
- 只能继承父类的实例属性

```
function Class() {
    // 只有超类中的参数顺序与子类中的参数顺序完全一致时才可以传递参数对象
    // 如果不是，就必须创建一个单独的数组，按照正确的顺序放置参数
    SubClass.apply(this, arguments);
}
```

### 2. 原型链继承

- 不支持多重继承
- 子类的实例所有的属性和方法都得去原型链上去找，找到的属性方法都是同一个。

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

Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
var cat = new Cat("大毛", "黄色");
console.log(cat.species); // 动物
console.log(cat._proto_ == Animal.prototytpe); // true
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

### 3. 组合式继承
- 使用对象冒充继承构造函数内的属性
- 用原型链继承 prototype 对象的方法
- 超类型在使用过程中会被调用两次

```
function ClassA(sColor) {
    this.color = sColor;
}

ClassA.prototype.sayColor = function () {
    alert(this.color);
};

function ClassB(sColor, sName) {
    ClassA.call(this, sColor); // 第二次
    this.name = sName;
}

ClassB.prototype = new ClassA(); //第一次调用

ClassB.prototype.sayName = function () {
    alert(this.name);
};
```
### 4. 寄生组合继承

```
function Person (name, age) {
            this.name = name;
            this.age = age;
}
Person.prototype.say = function(){
    console.log('hello, my name is ' + this.name);
};
function Man(name, age) {
    Person.apply(this, arguments);
}
Man.prototype = Object.create(Person.prototype);
Man.prototype.constructor = Man;
```

### 5. 拷贝继承

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

## 三 非构造函数继承

### 1. Object.create 方法(ECMAScript 5 中引入)

- 不论是原生的还是自定义的 Object.create ，其性能都远没有 new 的优化程度高，前者要比后者慢高达10倍。

```
// 实现
Object.create = function (parent) {
	function F() {};
    F.prototype = parent;
    return new F();
}

```

```
var Chinese = {
    nation: '中国'
};
var Doctor = Object.create(Chinese);
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
- [关于__proto__和prototype的一些理解](http://www.cnblogs.com/zzcflying/archive/2012/07/20/2601112.html)
- [Javascript面向对象编程（二）：构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)
- [ECMAScript 继承机制实现](http://www.w3school.com.cn/js/pro_js_inheritance_implementing.asp)



 





















