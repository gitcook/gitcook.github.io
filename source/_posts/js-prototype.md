---
title: JavaScript原型学习笔记
date: 2017-02-08 13:34:16
tags:
- javascript
- object
categories: FontEnd
---
Javascript是一种基于对象（object-based）的语言，你遇到的所有东西几乎都是对象。但是，它又不是一种真正的面向对象编程（OOP）语言，因为它的语法中没有`class`（类）。

JavaScript 不包含传统的类继承模型，而是使用 prototype原型模型。虽然这经常被当作是 JavaScript 的缺点被提及，其实基于原型的继承模型比传统的类继承还要强大。实现传统的类继承模型是很简单，但是实现 JavaScript 中的原型继承则要困难的多。


#### 什么是原型？
原型是一个对象，通过原型可以实现对象的属性继承。而对象是若干个无序属性的集合。对象的属性为字符串。
<!-- more -->
#### 那些对象有原型
所有的对象在默认的情况下都有一个原型，因为原型本身也是对象，所以每个原型自身又有一个原型(只有一种例外，默认的对象原型在原型链的顶端。即Object.protorype)。

#### 认识原型
在JavaScript的对象中都包含了一个”[[Prototype]]”内部属性，这个属性所对应的就是该对象的原型。“[[Prototype]]”作为对象的内部属性，是不能被直接访问的。所以为了方便查看一个对象的原型，Firefox和Chrome中提供了`__proto__`这个非标准（不是所有浏览器都支持）的访问器（ECMA引入了标准对象原型访问器`Object.getPrototypeOf(object)`）。在JavaScript的原型对象中，还包含一个`constructor`属性，这个属性对应创建所有指向该原型的实例的构造函数。

##### 方法
方法这个特殊的对象除了和其他对象一样有`__proto__`属性之外，还有自己特有的属性——原型属性（prototype）。这个属性是一个指针，指向一个对象。这个对象的用途是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做`constructor`，这个属性包含一个指针，指回原**构造函数**.

***任何一个对象都有`constructor`属性，指向创建这个对象的构造函数。***

***隐式原型（`__proto__`）指向创建这个对象的函数的prototype。***

``` bash
function Foo(y){
    this.y = y;// 构造函数将会以特定模式创建对象：被创建的对象都会有"y"属性
}

Foo.prototype.x = 10;

Foo.prototype.calculate = function(z) {
    return this.x + this.y + z;
};

var b = new Foo(20);
var c = new Foo(30);

console.log(b.calculate(30)); // 10 + 20 + 30 = 60
console.log(c.calculate(50)); // 10 + 30 + 50 = 90

// 让我们看看是否使用了预期的属性

console.log(

  b.__proto__ === Foo.prototype, // true
  c.__proto__ === Foo.prototype, // true

  // "Foo.prototype"自动创建了一个特殊的属性"constructor"
  // 指向a的构造函数本身
  // 实例"b"和"c"可以通过授权找到它并用以检测自己的构造函数

  b.constructor === Foo, // true
  c.constructor === Foo, // true
  Foo.prototype.constructor === Foo // true

  b.calculate === b.__proto__.calculate, // true
  b.__proto__.calculate === Foo.prototype.calculate // true

);

```
![原型](http://ojf9z9wko.bkt.clouddn.com/image/js-prototype.png)

##### 原型链
如果一个原型对象的原型不为null的话，我们就称之为原型链。

##### 属性查找
当查找一个对象的属性时，JavaScript 会向上遍历原型链，直到找到给定名称的属性为止，到查找到达原型链的顶部 - 也就是 Object.prototype - 但是仍然没有找到指定的属性，就会返回 undefined。例子：
``` bash
function foo() {
    this.add = function (x, y) {
        return x + y;
    }
}

foo.prototype.add = function (x, y) {
    return x + y + 10;
}

Object.prototype.subtract = function (x, y) {
    return x - y;
}

var f = new foo();
console.log(f.add(1, 2)); //结果是3，而不是13
console.log(f.subtract(1, 2)); //结果是-1

```
属性在查找的时候是先查找自身的属性，如果没有再查找原型，再没有，再往上走，一直查到Object的原型上。

再上一张图来表示`__proto__`和`prototype`的关系。

![原型链](http://ojf9z9wko.bkt.clouddn.com/image/js-prototype-chain.jpg)

参考文献
[http://www.nowamagic.net/librarys/veda/detail/1648](http://www.nowamagic.net/librarys/veda/detail/1648)
[http://www.jb51.net/article/80109.htm](http://www.jb51.net/article/80109.htm)