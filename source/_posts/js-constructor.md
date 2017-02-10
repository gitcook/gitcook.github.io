---
title: JavaScript构造函数学习笔记
date: 2017-02-10 13:57:04
tags:
- javascript
- object
categories: FontEnd
---
JavaScript 中的构造函数和其它语言中的构造函数是不同的。 通过`new`关键字方式调用的函数都被认为是构造函数。

#### 构造函数

``` javascript
function Person(name) {
    this.name = name;
}

var jack = new Person('杰克'); // 当这一句代码结束,你就可以肯定的认为 
                               // Person 函数是一个构造函数,因为它 new 了"杰克"
```

**不是所有函数都是构造函数, 只有 new 出实例的函数才是构造函数.**
<!-- more -->
实例对象`jack`被构造函数 Person 实例化之后,它就有一个属性`constructor`,此属性是一个指针,指向构造函数 (告诉我们是谁创造了它)同时,实例对象`jack`初始化以后同时有了一个只读属性`__proto__` ,这个指针指向了原型对象.

`jack.__proto__ === Person.prototype`这是一定成立的.

除了创建对象，构造函数(constructor) 还做了另一件有用的事情—自动为创建的新对象设置了原型对象(prototype object) 。原型对象存放于 ConstructorFunction.prototype 属性中。

原型介绍请看[JavaScript原型学习笔记](https://gitcook.github.io/2017/02/08/js-prototype/) 


参考资料
[https://bonsaiden.github.io/JavaScript-Garden/zh/#function.constructors](https://bonsaiden.github.io/JavaScript-Garden/zh/#function.constructors)
[http://www.nowamagic.net/librarys/veda/detail/1642](http://www.nowamagic.net/librarys/veda/detail/1642) 
[http://yijiebuyi.com/blog/7193e9e04fa01c4d7c05ec5b29494d9d.html](http://yijiebuyi.com/blog/7193e9e04fa01c4d7c05ec5b29494d9d.html) 