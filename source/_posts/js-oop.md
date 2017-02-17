---
title: JavaScript面向对象编程学习笔记
date: 2017-02-17 14:30:00
tags:
- javascript
- object
categories: FontEnd
---
对每个开发人员来说，面向对象编程都是必须要掌握的技能，但是JavaScript和其他语言不同，因为JavaScript本身不是面向对象的语言，而是基于对象的语言。而ECMAScript中面向对象编程的定义:
> ECMAScript是基于原型实现的面向对象编程语言。

在面向对象的语言中，我们使用类来创建一个自定义对象。然而JavaScript中几乎所有事物都是对象，那么用什么办法来创建自定义对象呢？

这就需要引入另外一个概念 - 原型（prototype），我们可以简单的把prototype看做是一个模版，新创建的自定义对象都是这个模版（prototype）的一个拷贝 （实际上不是拷贝而是链接，只不过这种链接是不可见，给人们的感觉好像是拷贝）。
<!-- more -->
### 实例对象

来看一个例子：
``` javascript
//构造函数
function Person(name, age, sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}

 // 定义Person的原型，原型中的属性可以被自定义对象引用
Person.prototype.getName = function() {
    return this.name;
};

Person.prototype.getAge = function() {
    return this.age;
};
// 注意如果用对象字面量的形式创建，`constructor`的指向会改变。
```
这里我们把函数Person称为构造函数，也就是创建自定义对象的函数。可以看出，JavaScript通过构造函数和原型的方式模拟实现了类的功能。 
创建自定义对象（实例化类）的代码：
``` javascript
var zhangsan = new Person('zhangsan', 25, 'male');
console.log(zhangsan.getName()); // zhangsan
console.log(zhangsan.constructor == Person); // true
```
Javascript规定，每一个构造函数都有一个`prototype`属性，指向另一个对象。这个对象的所有属性和方法，都会被构造函数的实例继承。而实例会有一个`constructor`属性指向它们的构造函数。

`isPrototypeOf()`这个方法可以来判断某个prototype对象和某个实例间的关系。
```
console.log(Person.prototype.isPrototypeOf(zhangsan)); // true
```

`hasOwnProperty()`用来判断某一个属性到底是本地属性，还是继承自prototype对象的属性。

```
console.log(zhangsan.hasOwnProperty('name')); // true
console.log(zhangsan.hasOwnProperty('getName')); // false
```

### 继承

现在有2个构造函数：
```
function Parent(name) {
    this.name = name;
}

function Child(age) {
    this.age = age;
}
```
如何让Child继承name？

#### 构造函数继承

使用call或apply方法，将父对象的构造函数绑定在子对象上。

``` javascript
function Parent(name) {
    this.name = name;
}

function Child(name, age) {
    Parent.call(this, name);
    this.age = age;
}

var anni = new Child('anni', 12);
anni.name // anni
```

#### prototype模式

如果Child的prototype对象，指向一个Parent的实例，那么所有Child的实例，就能继承Parent了。

``` javascript
Child.prototype = new Parent();
Child.prototype.constructor = Child;//让构造函数指回Child，而不是Parent
var jack = new Child('jack', 22);
jack.name ;// jack
```
#### 空对象中介

其实原理依然是prototype。直接继承至Parent.prototype。

``` javascript
function extend(Child, Parent) {
    var F = function(){};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
    Child.uber = Parent.prototype;
}
```
#### 非构造函数

``` javascript
function object(o) {
    function F() {}
    F.prototype = o;
    return new F();
}
```

#### 拷贝
``` javascript
function deepCopy(p, c) {
    var c = c || {};
    for (var i in p) {
        if (typeof p[i] === 'object') {
            c[i] = (p[i].constructor === Array) ? [] : {};
            deepCopy(p[i], c[i]);
        } else {
            c[i] = p[i];
           }
     }
    return c;
}
```

参考资料：
[http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html) 
[https://segmentfault.com/a/1190000002440502](https://segmentfault.com/a/1190000002440502) 
[http://www.cnblogs.com/sanshi/archive/2009/07/08/1519036.html](http://www.cnblogs.com/sanshi/archive/2009/07/08/1519036.html) 