---
title: CSSOM视图模式介绍
date: 2017-01-21 22:37:39
tags: javascript
categories: FontEnd
---

## Window视图属性
这些属性可以获取住整个浏览器窗体大小。
1. `innerWidth` 属性和 `innerHeight` 属性
2. `pageXOffset` 属性和 `pageYOffset` 属性
3. `screenX` 属性和 `screenY` 属性
4. `outerWidth` 属性和 `outerHeight` 属性

### innerWidth属性和innerHeight属性
`innerWidth`表示获取window窗体的内部宽度，不包括用户界面元素,浏览器窗口的尺寸（浏览器的视口，**不包括工具栏和滚动条**）。
<!-- more -->
示例代码：
``` bash
window.innerWidth
window.innerHeight
```
兼容性：不支持IE6/7/8，`innerWidth`和`innerHeight`属性只读，没有默认值。

### outerWidth属性和outerHeight属性
`outerWidth/outerHeight`表示整个浏览器窗体的大小，**包括所有界面元素（如工具栏/滚动条）。**

示例代码：
``` bash
window.outerWidth
window.outerHeight
```
兼容性：不支持IE6/7/8，`outerWidth`和`outerHeight`属性只读，没有默认值。

区别如图：
![](http://ojf9z9wko.bkt.clouddn.com/image/FirefoxInnerVsOuterHeight2.png)

### pageXOffset属性和pageYOffset属性
`pageXOffset`和`pageYOffset`，表示整个页面滚动的像素值（水平方向的和垂直方向的）。

示例代码：
``` bash
window.pageXOffset
window.pageYOffset
```
兼容性：不支持IE6/7/8，`pageXOffset`和`pageYOffset`属性只读，没有默认值。

### screenX属性和screenY属性
浏览器窗口在显示器中的位置，`screenX`表示水平位置，`screenY`表示垂直位置。

示例代码：
``` bash
window.screenX
window.screenY
```
兼容性：不支持IE6/7/8，IE低版本浏览器使用`window.screenLeft` 和 `window.screenTop`可获得相同的值。

## Screen视图属性
指能获取显示器信息的一些属性。
1. `availWidth` 属性和 `availHeight` 属性
2. `colorDepth` 属性
3. `pixelDepth` 属性
4. `width` 属性和 `height` 属性

### availWidth属性和availHeight属性
显示器可用宽高，不包括任务栏之类的。

示例代码：
``` bash
screen.avialWidth
screen.avialHeight
```
兼容性良好。

### colorDepth属性
表示显示器的颜色深度。

示例代码：
``` bash
screen.colorDepth
```
兼容性：主浏览器都支持返回24，IE6/7/8返回32。

### pixelDepth
该属性基本上与`colorDepth`一样。低版本IE不支持。

### width和height
表示显示器屏幕的宽高。

示例代码：
``` bash
screen.width
screen.height
```
兼容性良好。

## 文档视图(DocumentView)和元素视图(ElementView)方法
四个方法：
``` bash
elementFromPoint()
getBoundingClientRect()
getClientRects()
scrollIntoView()
```

### elementFromPoint()方法
返回给定坐标处所在的元素。

示例代码：
``` bash
document.elementFromPoint(x,y)
```
兼容性：不过位置坐标不太一样，浏览器还是有差异的。

### getBoundingClientRect()方法
得到矩形元素的界线，返回的是一个对象，包含`top`,`left`,`right`,和`bottom`四个属性值，大小都是相对于**文档视图左上角**计算而来。

示例代码：
``` bash
element.getBoudingClientRect()
```
兼容性良好。

### getClientRects()方法
返回元素的数个矩形区域，返回的结果是个对象列表，具有数组特性。这里的矩形选区只针对inline box，因此，只针对`a`,`span`,`em`这类标签元素，这个下面会详细讲述。

兼容性：IE6/7有bug，其他支持。

### scrollIntoView()方法
让元素滚动到可视区域（不属于草案方法），类似锚点跳转功能页面定位。

示例代码：
``` bash
element.scrollIntoView()
```
兼容性良好。

## 元素视图属性(Element)
关于元素大小位置等信息的一些属性。有：
`clientLeft`和`clientTop`
`clientWidth`和`clientHeight`
`offsetLeft`和`offsetTop`
`offsetParent`
`offsetWidth`和`offsetHeight`
`scrollLeft`和`scrollTop`
`scrollWidth`和`scrollHeight`

### clientLeft属性和clientTop属性
表示内容区域的左上角相对于整个元素左上角的位置（包括边框）。

示例代码：
``` bash
obj.clientLeft
obj.clientTop
```
兼容性良好。

### clientWidth属性和clientHeight属性
表示内容区域的高度和宽度，包括padding大小，但是不包括边框和滚动条。

示例代码：
``` bash
obj.clientWidth
obj.clientHeight
```
兼容性良好。

### offsetLeft属性和offsetTop属性
表示相对于最近的祖先定位元素（CSS position 属性被设置为`relative`、`absolute`或`fixed`的元素）的左右偏移值。

``` bash
obj.offsetLeft
obj.offsetTop
```
兼容性良好。

### offsetParent属性
第一个祖定位元素（即用来计算上面的offsetLeft和offsetTop的元素）。
offsetParent元素只可能是下面这几种情况：
> * `<body>`
> * `position`不是`static`的元素
> * `<table>`,`<th>`或`<td>`，但必须要`position: static`。

``` bash
obj.offsetParent
```
兼容性良好。

### offsetWidth属性和offsetHeight属性
整个元素的尺寸（包括边框）。

示例代码：
``` bash
obj.offsetWidth
obj.offsetHeight
```
兼容性良好。

### scrollLeft属性和scrollTop属性
表示元素滚动的像素大小。

示例代码：
``` bash
obj.scrollLeft
obj.scrollTop
```
兼容性良好。

例子：元素位置(绝对)
``` bash
obj.getBoundingClientRect().left + Math.max(document.documentElement.scrollLeft, document.body.scrollLeft);
obj.getBoundingClientRect().top + Math.max(document.documentElement.scrollTop, document.body.scrollTop);
```
### scrollWidth属性和scrollHeight属性
表示整个内容区域的宽高，包括隐藏的部分。如果元素没有隐藏的部分，则相关的值应该等用于`clientWidth`和`clientHeight`。当你向下滚动滚动条的时候，`scrollHeight`应该等用于`scrollTop + clientHeight`。

兼容性：不理想。

## 鼠标位置(Mouse position)
相关的些属性有：
`clientX`,`clientY`
`offsetX`,`offsetY`
`pageX`,`pageY`
`screenX`,`screenY`
`x`,`y`

### clientX和clientY
相对于window，为鼠标相对于window的偏移,**不包括滚动条**。

示例代码：
``` bash
event.clientX
event.clientY
```
兼容性良好。

### offsetX和offsetY
表示鼠标相对于当前被点击元素`padding box`的左上偏移值。

示例代码：
``` bash
event.offsetX
event.offsetY
```
兼容性：不理想。

### pageX和pageY
为鼠标相对于document的坐标。**包括滚动条的偏移**。

示例代码：
``` bash
event.pageX
event.pageY
```
兼容性：IE6/7/8不支持。

### screenX和screenY
鼠标相对于显示器屏幕的偏移坐标。

示例代码：
``` bash
event.screenX
event.screenY
```
兼容性良好。


## 结语
上面陆续出现的属性或方法，那些兼容性不错的东西我们可以多多关注下。很多特性都是非常实用的。
例如：
``` bash
getBoundingClientRect()
scrollIntoView()
clientWidth和clientHeight
offsetLeft和offsetTop
offsetWidth和offsetHeight
scrollLeft和scrollTop
clientX和clientY
```