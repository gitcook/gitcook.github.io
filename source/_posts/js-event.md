---
title: JavaScript事件绑定、监听及代理学习笔记
date: 2017-03-05 13:14:52
tags:
- javascript
- event
categories: FontEnd
---

#### 事件
在JavaScript的学习中，我们经常会遇到JavaScript的事件机制，什么事件捕获、事件冒泡和事件代理（委托）各种专业名词。可以说事件机制在JavaScript中是非常重要的内容，这里就让我们来详细了解下事件机制。

#### 事件绑定
##### 在DOM中绑定事件
<!-- more -->
DOM0
``` javascript
element.onclick = function(e) {};
// 或者
element["onclick"] = function(e) {};

// 其中e表示事件对象

ele.onclick = null; // 消除事件
```
对于同一个dom节点而言，只能注册一个，后边注册的**同种**事件会覆盖之前注册的。

DOM2
 1. 支持同一dom元素注册多个同种事件。
 2. 事件捕获和事件冒泡

方法：`element.addEventListener(event, listener, useCapture)` 和 `element.removeEventListener(event, listener, useCapture)`

`event` : （必需）事件名，支持所有DOM事件。
`listener`：（必需）指定要事件触发时执行的函数。
`useCapture`：（可选）指定事件是否在捕获或冒泡阶段执行。true，捕获。false，冒泡。默认false。

IE: (event 需要加'on')
`ele.attachEvent(event, listener)`
`ele.detachEvent(event, listener)`

``` javascript
ele.addEventListener('click', fn, false);
ele.removeEventListener('click', fn, false);
// remove的useCapture必须和add的一样

ele.attachEvent('onclick', fn);
ele.detachEvent('onclick', fn);
```
##### 封装事件监听
``` javascript
//绑定监听事件
function addEventHandler(element, event, listener) {
    if (element.addEventListener) {
        element.addEventListener(event, listener, false);
    }
    else if (element.attachEvent) {
        element.attachEvent("on" + event, listener);
    }
    else {
        element['on' + event] = listener;
    }
}

//移除监听事件
function removeEventHandler(element, event, listener){
    if (element.removeEventListener) {
        element.removeEventListener(event, listener, false);
    }
    else if (element.detachEvent) {
        element.detachEvent("on" + event, listener);
    }
    else {
        element['on' + event] = null;
    }
}
```
#### 事件的捕获和冒泡
![事件阶段](http://ojf9z9wko.bkt.clouddn.com/image/js-evet.png)

W3C规范中定义了3个事件阶段，依次是捕获阶段、目标阶段、冒泡阶段。

规律：在一个元素上既有捕获又有冒泡，当事件触发时，如果触发元素就是此元素，那么会先捕获后冒泡；如果触发元素是此元素的子元素，那么会按照注册顺序执行。

[demo](https://gitcook.github.io/demo/event.html)

**取消冒泡**
`event.stopPropagation()`, IE 是`event.cancelBubble = true`

#### 事件代理（委托）
1. 父元素绑定事件。
2. 父元素知道事件的实际发生目标是谁。
3. 我们要对目标进行判断，如果是我们需要的元素，则发生回调函数。
[demo](https://gitcook.github.io/demo/ife-201703/binbin/js-task04.html)
``` javascript
main.onclick = function () {
    var e = arguments[0] || window.event;
    var target = e.target || e.srcElement;
    if (target.nodeName.toLowerCase() === 'button') {
        if (target.id === 'left-in') {
            fn();
        }
        if (target.id === 'right-in') {
            fn();
        }
        if (target.id === 'left-out') {
            fn();
        }
        if (target.id === 'right-out') {
            fn();
        }
    }
    else if (target.nodeName.toLowerCase() === 'li') {
        fn1();
    }
};

// 或者
main.addEventListener('click', fn, false);
```
2017-3-12 更新
1. 通常支持事件冒泡（Event Bubbling）的事件类型为鼠标事件和键盘事件，例如：mouseover, mouseout, click, keydown, keypress。
2. 接口事件则通常不支持事件冒泡（Event Bubbling），例如：load, change, submit, focus, blur。

对于这些事件的代理可以使用事件捕获来处理。
```
ele.addEventListener('focus', fn, true);
ele.addEventListener('blur', fn, true);
// IE
ele.onfocusin = fn();
ele.onfocusout = fn();
```
[代理所有input的focus和blur事件](https://gitcook.github.io/demo/ife-201703/yaoyao/demo2.html)
参考资料:
1. [http://www.cnblogs.com/rubylouvre/archive/2009/08/09/1542174.html](http://www.cnblogs.com/rubylouvre/archive/2009/08/09/1542174.html)
2. [http://www.cnblogs.com/lhb25/archive/2012/11/30/oninput-and-onpropertychange-event-for-input.html](http://www.cnblogs.com/lhb25/archive/2012/11/30/oninput-and-onpropertychange-event-for-input.html)
3. [http://yujiangshui.com/javascript-event/](http://yujiangshui.com/javascript-event/)