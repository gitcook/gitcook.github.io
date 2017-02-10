---
title: JavaScript闭包学习笔记
date: 2017-02-10 16:45:39
tags:
- javascript
- object
categories: FontEnd
---
闭包是 JavaScript 一个非常重要的特性，这意味着当前作用域总是能够访问外部作用域中的变量。 因为函数是 JavaScript 中唯一拥有自身作用域的结构，因此闭包的创建依赖于函数。

#### 什么是闭包
闭包的定义非常晦涩
> 闭包，是指语法域位于某个特定的区域，具有持续参照（读写）位于该区域内自身范围之外的执行域上的非持久型变量值能力的段落。这些外部执行域的非持久型变量神奇地保留它们在闭包最初定义（或创建）时的值（深连结）。

简单来说，闭包就是在另一个作用域中保存了一份它从上一级函数或作用域取得的变量（键值对），而这些键值对是不会随上一级函数的执行完成而销毁。
<!-- more -->
如果一个函数访问了它的外部变量，那么它就是一个闭包。

注意，外部函数不是必需的。通过访问外部变量，一个闭包可以维持（keep alive）这些变量。在内部函数和外部函数的例子中，外部函数可以创建局部变量，并且最终退出；但是，如果任何一个或多个内部函数在它退出后却没有退出，那么内部函数就维持了外部函数的局部数据。

#### 闭包的作用
* 可以读取函数内部的变量。(保护函数内的变量安全)
* 让这些变量的值始终保持在内存中。

``` javascript
function makeAdder(x) {
    return function(y) {
        return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```
add5 和 add10 都是闭包。它们共享相同的函数定义，但是保存了不同的环境。在 add5 的环境中，x 为 5。而在 add10 中，x 则为 10。

makeAdder执行完以后变量x的值始终存在内存中，没有被回收。内部函数运行依赖外部函数的变量x。

例子：
``` javascript
var singleton = function () {
    var privateVariable;
    function privateFunction(x) {
        ...privateVariable...
    }
 
    return {
        firstMethod: function (a, b) {
            ...privateVariable...
        },
        secondMethod: function (c) {
            ...privateFunction()...
        }
    };
}();
```
这个单件通过闭包来实现。通过闭包完成了私有的成员和方法的封装。匿名主函数返回一个对象。对象包含了两个方法，方法1可以方法私有变量，方法2访 问内部私有函数。需要注意的地方是匿名主函数结束的地方的’()’，如果没有这个’()’就不能产生单件。因为匿名函数只能返回了唯一的对象，而且不能被 其他地方调用。这个就是利用闭包产生单件的方法。

理解JavaScript的闭包是迈向高级JS程序员的必经之路，理解了其解释和运行机制才能写出更为安全和优雅的代码。

参考资料
[http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html) 
[http://www.oschina.net/question/28_41112](http://www.oschina.net/question/28_41112) 
[http://www.jb51.net/article/18303.htm](http://www.jb51.net/article/18303.htm) 