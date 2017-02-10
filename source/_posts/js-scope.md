---
title: JavaScript作用域学习笔记
date: 2017-02-09 13:17:16
tags:
- javascript
- object
categories: FontEnd
---
作用域是JavaScript最重要的概念之一，想要学好JavaScript就需要理解JavaScript作用域和作用域链的工作原理。

#### JavaScript作用域
任何程序设计语言都有作用域的概念，简单的说，作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期。

#### 变量作用域
不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象`window`，**全局作用域的变量实际上被绑定到`window`的一个属性。**
<!-- more -->
在ES6之前，局部变量只能存在于函数体中，并且函数的每次调用它们都拥有不同的作用域范围。 局部变量只能在其被调用期的作用域范围内被赋值、检索、操纵。

需要注意，在ES6之前，JavaScript不支持块级作用域，这意味着在`if`语句、`switch`语句、`for`循环、`while`循环中无法支持块级作用域。 也就是说，ES6之前的JavaScript并不能构建类似于Java中的那样的块级作用域（变量不能在语句块外被访问到）。但是， 从ES6开始，你可以通过`let`关键字来定义变量，它修正了`var`关键字的缺点，能够让你像Java语言那样定义变量，并且支持块级作用域。

#### JavaScript的作用域链

JS权威指南中有一句很精辟的描述:　**”JavaScript中的函数运行在它们被定义的作用域里,而不是它们被执行的作用域里.”**

``` javascript
var name = 'laruence';
function echo() {
    console.log(name);
}

function env() {
    var name = 'eve';
    echo();
}

env(); // laruence
```

**作用域链是一个对象列表或链表。这组对象定义了这段代码“作用域中”的变量。**

在JS中，作用域的概念和其他语言差不多， 在每次调用一个函数的时候 ，就会进入一个函数内的作用域，当从函数返回以后，就返回调用前的作用域.

JS的语法风格和C/C++类似, 但作用域的实现却和C/C++不同，并非用“堆栈”方式，而是使用列表，具体过程如下(ECMA262中所述):

* 任何执行上下文时刻的作用域, 都是由作用域链(scope chain)来实现。
* 在一个函数被定义的时候, 会将它定义时刻的scope chain链接到这个函数对象的[[scope]]属性。
* 在一个函数对象被调用的时候，会创建一个活动对象(也就是一个对象), 然后对于每一个函数的形参，都命名为该活动对象的属性, 然后将这个活动对象做为此时的作用域链(scope chain)**最前端**, 并将这个函数对象的[[scope]]加入到scope chain中。

``` javascript
function add(num1,num2) {
    var sum = num1 + num2;
    return sum;
}

var total = add(5,10);
```

![js-scope-chain](http://ojf9z9wko.bkt.clouddn.com/image/js-scope-chain.jpg)

**注意，全局执行环境的变量对象(window)始终都是作用域链的最后一个对象。**

在函数执行过程中，每遇到一个变量，都会经历一次标识符解析过程以决定从哪里获取和存储数据。该过程从作用域链头部，也就是从活动对象开始搜索，查找同名的标识符，如果找到了就使用这个标识符对应的变量，如果没找到继续搜索作用域链中的下一个对象，如果搜索完所有对象都未找到，则认为该标识符未定义。函数执行过程中，每个标识符都要经历这样的搜索过程。

参考资料
[http://www.cnblogs.com/lhb25/archive/2011/09/06/javascript-scope-chain.html](http://www.cnblogs.com/lhb25/archive/2011/09/06/javascript-scope-chain.html)
[http://www.laruence.com/2009/05/28/863.html](http://www.laruence.com/2009/05/28/863.html)
[http://wwsun.github.io/posts/scope-and-context-in-javascript.html](http://wwsun.github.io/posts/scope-and-context-in-javascript.html)
