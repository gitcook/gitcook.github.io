---
title: JavaScript数据结构与算法之二叉搜索树
date: 2017-03-11 11:36:26
tags:
- javascript
- data
categories: FontEnd
---
在上一节里我简单介绍了树形结构以及二叉树，这一节就来了解下二叉搜索树（一种特殊的二叉树），在实际应用中二叉搜索树的应用广泛。

#### 二叉搜索树
二叉搜索树：它只允许你在左侧节点储存比父节点小的值，右侧只允许储存比父节点大的值。
<!-- more -->
函数构建代码：(普通二叉树类似，利用random函数随机加入左右)
``` javascript
// 构造函数
function BST() {
    this.root = null;
    this.Node = function(key) {
        this.value = key; // 最好把key处理为数字（懒的写了）
        this.left = null;
        this.right = null;
    };

    // add node
    this.insert = function(key) {
        // 新建节点
        key = new this.Node(key);
        if (!this.root) {
            this.root = key;
        }
        else { // 根存在
            insertNode(this.root, key);
        }

        function insertNode(parentNode, newNode) {
            if (newNode.value < parentNode.value) { // 小于父节点
                if (!parentNode.left) { // 左子节点不存在
                    parentNode.left = newNode;
                }
                else { // 左子节点存在
                    parentNode = parentNode.left; // 父节点换成当前的左节点
                    return insertNode(parentNode, newNode); // 递归处理
                }
            }
            else { // 大于父节点
                if (!parentNode.right) {
                    parentNode.right = newNode;
                }
                else {
                    parentNode = parentNode.right;
                    return insertNode(parentNode, newNode);
                }
            }
        }
        return this.root;
    };

    // 查找
    this.find = function(key) {
        // 不新建节点也可以，下面比较时就要区分
        key = new this.Node(key);
        if (!this.root) {
            console.log('root不存在');
            return false;
        }
        else { // 根存在
            findNode(this.root, key);
        }

        function findNode(parentNode, newNode) {
            if (newNode.value < parentNode.value) { // 小于递归查找左边
                findNode(parentNode.left, newNode);
            }
            else if (newNode.value > parentNode.value) { // 大于递归查找右边
                findNode(parentNode.right, newNode);
            }
            else { // 等于直接输出
                console.log(parentNode);
                return true;
            }
        }
    };

    // 删除
    this.delete = function(key) {
        key = new this.Node(key);
        if (!this.root) {
            console.log('root不存在');
            return false;
        }
        else {
            delNode(this.root, key);
        }

        function delNode(parentNode, newNode) {
            if (newNode.value < parentNode.value) { // 小于比较左边
                if (!parentNode.left){ // 左边不存在
                    return false;
                }
                else { // 左边存在(1.等于输出2.不等于递归)
                    if (parentNode.left.value === newNode.value) {
                        parentNode.left = null;
                        return true;
                    }
                    else {
                        delNode(parentNode.left, newNode);
                    }
                }
            }
            else if (newNode.value > parent.value){ // 大于比较右边
                if (!parentNode.right) {
                    return false;
                }
                else {
                    if (parentNode.right.value === newNode.value) {
                        parentNode.right = null;
                        return true;
                    }
                    else {
                        delNode(parentNode.right, newNode);
                    }
                }
            }
            else { // 等于直接赋值为空
                parentNode = null;
                return true;
            }
        }
    };
    // 如何遍历上一节已经介绍了，这里就不写了，也可以添加其他例如max、min方法。
    this.max = function() {
        return maxNode(this.root);

        function maxNode(node) {
            if (node) {
                while(node.right) {
                    node = node.right;
                }
                return node.value;
            }
            return false;
        }
    };

    this.min = function() {
        return minNode(this.root);

        function minNode(node) {
            if (node) {
                while(node.left) {
                    node = node.left;
                }
                return node.value;
            }
            return false;
        }
    };

}

// 测试
var tree = new BST();
console.time('insert test');
tree.insert(75);
tree.insert(24);
tree.insert(35);
tree.insert(21);
tree.insert(67);
tree.insert(78);
tree.insert(85);
tree.insert(61);
tree.insert(12);
tree.insert(59);
tree.insert(62);
tree.insert(55);
tree.insert(53);
console.timeEnd('insert test');
console.time('find test');
tree.find(61);
console.timeEnd('find test');
console.time('delete test');
tree.delete(55);
console.timeEnd('delete test');

```

参考资料：
1. [https://segmentfault.com/a/1190000008244508](https://segmentfault.com/a/1190000008244508)
2. [https://github.com/qqcome110/web/issues/5](https://github.com/qqcome110/web/issues/5)
3. [http://www.imooc.com/article/4713](http://www.imooc.com/article/4713)