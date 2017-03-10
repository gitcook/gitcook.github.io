---
title: JavaScript数据结构与算法之--树的学习
date: 2017-03-10 17:45:51
tags:
- javascript
- data
categoried: FontEnd
---
最近刷BaiDu-IFE-2017的JavaScript练习了解了树形结构的知识。这里就做一点笔记，作为自己学习的记录。

#### 树
在数据结构中,使用树形结构表示数据之间一对多的关系,树形结构是一种非线型结构.
定义:
> 树(Tree)是n(n≥0)个相同数据类型的数据元素的集合.树中的数据元素称为节点(Node)。n=0的树称为空树(Empty Tree)；对于n＞0的任意非空树T有：
1. 有且仅有一个特殊的结点称为树的根(Root)结点，根没有前驱结点；
2. 若n＞1，则除根结点外，其余结点被分成了m(m＞0)个互不相交的集合T1，T2，…，Tm，其中每一个集合Ti(1≤i≤m)本身又是一棵树。树T1，T2，…，Tm称为这棵树的子树(Subtree)。
<!-- more -->
由树的定义可知，树的定义是递归的，用树来定义树。因此，树（以及二叉树）的许多算法都使用了递归。

树的常用术语：
1. 结点(Node)：表示树中的数据元素。
2. 结点的度(Degree of Node): 节点所拥有的子树的个数。
3. 树的度(Degree of Tree): 树中各节点度的最大值。
4. 叶子结点(Leaf Node)：度为0的结点，也叫终端结点。
5. 分支结点(Branch Node): 度不为0的节点。
6. 树的深度(Depth of Tree)：树中节点的最大层次数。


#### 二叉树
树中的每个节点至多能有两个子节点（树的度不大于2）。二叉树有左右之分，不可颠倒。如果把左右节点顺序颠倒则变了另一个全新的二叉树。

有一个二叉树式的html文档，如何遍历：
[Demo](https://gitcook.github.io/demo/ife-201703/binbin/js-task07.html)
##### 先序遍历(根左右)
``` javascript
function preOrder(node) {
    if (node) {
        console.log(node);
        preOrder(node.firstElementChild);
        preOrder(node.lastElementChild);
    }
}

// 二叉树对象
var preOrder = function (node) {
  if (node) {
    console.log(node.value);
    preOrder(node.left);
    preOrder(node.right);
  }
}
```
##### 中序遍历(左根右)
``` javascript
function inOrder(node) {
    if (node) {
        inOrder(node.firstElementChild);
        console.log(node);
        inOrder(node.lastElementChild);
    }
}

// 二叉树对象
var inOrder = function (node) {
  if (node) {
    inOrder(node.left);
    console.log(node.value);
    inOrder(node.right);
  }
}
```
##### 后序遍历(左右根)
``` javascript
function postOrder(node) {
    if (node) {
        postOrder(node.firstElementChild);
        postOrder(node.lastElementChild);
        console.log(node);
    }
}

// 二叉树对象
var postOrder = function (node) {
  if (node) {
    postOrder(node.left);
    postOrder(node.right);
    console.log(node.value);
  }
}
```

[Demo](https://gitcook.github.io/demo/ife-201703/binbin/js-task08.html)
##### 广度遍历(**遍历DOM树而来**)
``` javascript
//广度
function sco(node) {
    if (node) {
        var stack = [node];
        var target;
        while(stack.length) {
            target = stack.shift();
            arr.push(target);
            Array.prototype.push.apply(stack, target.children);
            // 直接stack.push(target.children)会报错,target.children 是一个数组
        }
    }
}
```

##### 深度遍历(其实效果和先序遍历是一样的)
**改变unshift,元素从前面加入到stack里面，而取出来的是第一个**
``` javascript
//深度(循环)
function dep(node) {
    if (node) {
        var stack = [node];
        var target;
        while(stack.length) {
            target = stack.shift(); // 取第一个元素
            arr.push(target);
            Array.prototype.unshift.apply(stack, target.children);
        }
    }
}

// 其他写法（childNodes和firstElementChild的用法）
// 递归
function rec(node) {
    if(node && node.nodeType === 1){
        console.log(node);
    }
    var i = 0, childNodes = node.childNodes;
    for(; i < childNodes.length ; i++){
        node = childNodes[i];
        if(node.nodeType === 1){
            //递归先序遍历子节点
            rec(node);
        }
    }
}

//递归另一种
function walkDom(node, callback) {
    if (!node) { //判断node是否为null
        return
    }
    callback(node) //将node自身传入callback
    node = node.firstElementChild //改变node为其子元素节点
    while (node) {
        walkDom(node, callback) //如果存在子元素，则递归调用walkDom
        node = node.nextElementSibling //从头到尾遍历元素节点
    }
}
```

参考资料:
1. [https://github.com/Lucifier129/Lucifier129.github.io/issues/4](https://github.com/Lucifier129/Lucifier129.github.io/issues/4)
2. [https://segmentfault.com/a/1190000004620352](https://segmentfault.com/a/1190000004620352)
3. [https://segmentfault.com/a/1190000000740261](https://segmentfault.com/a/1190000000740261)
4. [https://juejin.im/entry/5847c17a128fe10058bcf2c5](https://juejin.im/entry/5847c17a128fe10058bcf2c5)