---
title:      JavaScript 对象创建与继承
summary:    对象创建、继承方法总结
categories: JavaScript
tags:       JavaScript 学习笔记
---

## 对象创建 

### 1. Object构造函数、对象字面量

比较适用于创建单个对象

```javascript
// Object 构造函数
var person = new Object();
// 对象字面量
var person = {};
```

### 2. 工厂模式

无法实行对象识别，即无法知道对象的类型。

```javascript
function createPerson(name, age, job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };    
    return o;
}

var person1 = createPerson("Nicholas", 29, "Software Engineer");
```

### 3. 构造函数模式

没有实现函数复用。

```javascript
function Person(name) {
    this.name = name;
}
var p = new Person();
```

new 操作符的执行过程

```
// 创建一个新对象
var obj = {}; 
// 实现这个新对象的一个内部属性[[prototype]]，Firefox、Safari、Chrome实现为__proto__
obj.__proto__ = Person.prototype;
// 在obj上执行构造函数中的代码（为这个新对象添加属性）
Person.call(obj);
// 返回这个新对象
return obj;
```

### 4. 原型模式

在原型上添加所有实例共享的的属性和方法，原型的变化会在实例中反映出来。

```javascript
function Person(){
}

Person.prototype.sayName = function(){
    alert(this.name);
};
```

当属性较多时可以直接重写prototype对象，但这样会切断构造函数与最初原型之间的联系。

在重写之前创建的实例的[[prototype]]属性仍然指向最初原型，obj.constructor === A 的值也仍然是true，而在重写之后创建的实例的[[prototype]]属性指向新的原型，并且constructor属性因为重写而将直接指向`function Object() {}`，而不再是A，因此最好在重写时显式设置为A。

```javascript
function Person(){
}

var friend = new Person();

Person.prototype = {
    // constructor: Person,
    name : "Nicholas",
    age : 29,
    job : "Software Engineer",
    sayName : function () {
        alert(this.name);
    }
};

friend.sayName();   //error

console.log(friend.constructor === Person); //true
var brother = new Person();
console.log(brother.constructor === Person); // false
```

### 5. 组合使用构造函数模式和原型模式

构造函数用于定义实例属性，原型模式用于定义方法和共享的属性，是最常见的方式。

```javascript
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby", "Court"];
}

Person.prototype = {
    constructor: Person,
    sayName : function () {
        alert(this.name);
    }
};

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.friends.push("Van");

alert(person1.friends);    //"Shelby,Court,Van"
alert(person2.friends);    //"Shelby,Court"
alert(person1.friends === person2.friends);  //false
alert(person1.sayName === person2.sayName);  //true
```

### 6. 动态原型模式

在构造函数中初始化原型，并且在初始化前做一个判断。

```javascript
function Person(name, age, job){

    //properties
    this.name = name;
    this.age = age;
    this.job = job;
    
    //methods
    if (typeof this.sayName != "function"){
    
        Person.prototype.sayName = function(){
            alert(this.name);
        };
        
    }
}
```

### 7. 寄生构造函数模式

除了使用new操作符并把使用的函数叫做构造函数之外，这个模式和工厂模式其实是一模一样的。

```javascript
function Person(name, age, job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };    
    return o;
}

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();  //"Nicholas"
```

实例：创建一个具有额外方法的特殊数组

```
function SpecialArray(){       
    var values = new Array();
    
    values.push.apply(values, arguments);

    values.toPipedString = function(){
        return this.join("|");
    };
    
    return values;        
}

var colors = new SpecialArray("red", "blue", "green");
console.log(colors.toPipedString()); //"red|blue|green"

console.log(colors instanceof SpecialArray); // false
```

### 8. 稳妥构造函数模式

稳妥对象指的是没有公共属性，而且其方法也不引用this的对象。与寄生构造函数相似，只是创建的是稳妥对象。

```javascript
function Person(name, age, job) {
    var o = new Object();
    o.sayName = function () {
        alert(name);
    };
    return o;
}
```

除了调用sayName()方法外，没有别的方式可以访问其数据成员。

## 对象继承

### 1. 原型链继承

- 子类的 prototype 属性指向父类的实例
- 父类的实例属性和原型属性都被继承
- 父类中的实例属性变成了子类的原型属性
- 原型属性中的引用类型值被所有实例共享
- 不支持多重继承

```
Class.prototype = new SubClass();
// 避免继承链的紊乱
Class.prototype.constructor = Class;
```

```javascript
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
console.log(cat instanceof Animal); // true
```

### 2. 借用构造函数

- 可以实现多重继承
- 对于同名属性或方法，后继承的类具有高优先级
- 只能继承父类的实例属性

```javascript
function Class() {
    // 只有超类中的参数顺序与子类中的参数顺序完全一致时才可以传递参数对象
    // 如果不是，就必须创建一个单独的数组，按照正确的顺序放置参数
    SubClass.apply(this, arguments);
}
```

### 3. 组合继承
- 使用原型链实现对原型属性和方法的继承
- 通过借用构造函数实现对实例属性的继承
- 父类的构造函数在使用过程中会被调用两次，原型和实例中有重名属性。

```javascript
function ClassA(sColor) {
    this.color = sColor;
}

ClassA.prototype.sayColor = function () {
    alert(this.color);
};

function ClassB(sColor, sName) {
    ClassA.call(this, sColor); // 第二次 调用后实例包含属性color，值为sColor，覆盖了原型上的同名属性
    this.name = sName;
}

ClassB.prototype = new ClassA(); //第一次调用 子类原型对象存在一个color属性，值为undefined

ClassB.prototype.sayName = function () {
    alert(this.name);
};
```

### 4. 原型式继承

```javascript
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}
```

ECMAScript 5 新增了Object.create() 方法规范化了原型式继承。

```javascript
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};

// 第二个参数可选
var anotherPerson = Object.create(person, {
    name: {
        value: "Greg"
    }
});
```

### 5.寄生式继承

思路与寄生构造函数及工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象。

```javascript
function createAnother(original) {
	var clone = object(original);
	clone.sayHi = function() {
		alert('Hi');
	}
	return clone;
}
```

### 4. 寄生组合式继承

寄生组合式继承是对组合式继承的改进。组合式继承将子类原型设置为父类的实例，其中包含了父类的原型属性和实例属性，而实际上我们的根本目的只是需要将父类的原型属性复制给子类，这便是寄生组合式继承的原理。

```javascript
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}

function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype);
    prototype.constructor = subType;
    subType.prototype = prototype;
}

function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function(){
    console.log(this.name);
};

function SubType(name, age){  
  	// 实现实例属性的继承
    SuperType.call(this, name);
    
    this.age = age;
}

// 实现原型属性的继承
inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function(){
    console.log(this.age);
};

var instance1 = new SubType("Nicholas", 29);
console.log(instance1);
instance1.colors.push("black");
console.log(instance1.colors);  //"red,blue,green,black"
instance1.sayName();      //"Nicholas";
instance1.sayAge();       //29


var instance2 = new SubType("Greg", 27);
console.log(instance2.colors);  //"red,blue,green"
instance2.sayName();      //"Greg";
instance2.sayAge();       //27
```

## 参考目录

- `JavaScript高级程序设计（第三版）- 第六章 面向对象的程序设计`
- [关于__proto__和prototype的一些理解](http://www.cnblogs.com/zzcflying/archive/2012/07/20/2601112.html)
- [Javascript面向对象编程（二）：构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)
- [ECMAScript 继承机制实现](http://www.w3school.com.cn/js/pro_js_inheritance_implementing.asp)