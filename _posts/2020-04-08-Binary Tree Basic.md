---
layout: post
title: 二叉树基本操作
tags: [Programming]
description: 二叉树常用操作
keywords: Binary Tree, Data Structure, Algorithm
mathjax: false
comment: true
---

记录一下二叉树的基本常用操作，主要是前中后序遍历的递归/非递归解法、层序遍历的非递归解法以及如何根据前中后序确定二叉树。

---

## 数据结构定义

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val{x}, left{nullptr}, right{nullptr} {}
};
```

## 遍历

二叉树一般有四种遍历：前序、中序、后序和层序。

前中后序一般有递归和非递归两种实现方法，由于它们访问节点的路径是相同的，只是访问顺序不一样，因此这三种递归实现思路比较类似，而非递归实现则需要借助栈来实现（需要保存路径上经过但还未访问的节点）。

![前中后序遍历访问路径/访问顺序](/assets/images/2020-04-08-01.png)_图 1 前中后序遍历访问路径/访问顺序_

层序遍历通常使用非递归实现，从它的访问顺序来看它也并不适合递归实现，层序遍历借助队列来实现（保存未来将要访问的节点，即下一层的孩子节点）。

### 前序遍历

#### 递归实现

先访问根节点，然后依次访问左子树和右子树。

```cpp
void preorder_rec(TreeNode *root) {
    if (root) {
        std::cout << root->data << " ";
        preorder_rec(root->left);
        preorder_rec(root->right);
    }
}
```

#### 非递归实现

使用栈保存路径上经过的节点（并访问输出），向左访问到最左子节点后弹出，然后访问右子树。

```cpp
void preorder_nonrec(TreeNode *root) {
    std::stack<TreeNode *> nodes;
    while (root || !nodes.empty()) {
        while (root) {
            std::cout << root->data << " ";
            nodes.push(root);
            root = root->left;
        }
        root = nodes.top();
        nodes.pop();
        root = root->right;
    }
}
```

### 中序遍历

#### 递归实现

先访问左子树，然后访问根节点，最后访问右子树。

```cpp
void inorder_rec(TreeNode *root) {
    if (root) {
        inorder_rec(root->left);
        std::cout << root->data << " ";
        inorder_rec(root->right);
    }
}
```

#### 非递归实现

跟前序非递归类似，但访问时机不一样。使用栈保存路径上经过的节点，向左访问到最左子节点后弹出（并访问输出），然后访问右子树。

```cpp
void inorder_nonrec(TreeNode *root) {
    std::stack<TreeNode *> nodes;
    while (root || !nodes.empty()) {
        while (root) {
            nodes.push(root);
            root = root->left;
        }
        root = nodes.top();
        nodes.pop();
        std::cout << root->val << " ";
        root = root->right;
    }
}
```

### 后序遍历

#### 递归实现

先访问左子树，然后访问右子树，最后访问根节点。

```cpp
void postorder_rec(TreeNode *root) {
    if (root) {
        postorder_rec(root->left);
        postorder_rec(root->right);
        std::cout << root->val << " ";
    }
}
```

#### 非递归实现

由于根节点需要在右子树访问完之后才访问，因此需要用一个变量保存上一次访问的节点。

```cpp
void postorder_nonrec(TreeNode *root) {
    TreeNode *last_node = nullptr;
    std::stack<TreeNode *> nodes;
    while (root || !nodes.empty()) {
        while (root) {
            nodes.push(root);
            root = root->left;
        }
        root = nodes.top();
        nodes.pop();
        if (root->right == last_node || root->right == nullptr) {
            // 右子树已经被访问过或者为空则直接访问当前节点
            std::cout << root->val << " ";
            last_node = root;
            root = nullptr;
        } else {
            // 如果右子树还未访问则将当前节点重新压栈然后去访问右子树
            nodes.push(root);
            root = root->right;
        }
    }
}
```

### 层序遍历

层序遍历的规则显然非常适合使用 `BFS`，因此我们使用队列来完成遍历。

```cpp
void levelorder(TreeNode *root) {
    if (root) {
        std::queue<TreeNode *> nodes;
        nodes.push(root);
        while (!nodes.empty()) {
            root = nodes.front();
            nodes.pop();
            std::cout << root->val << " ";
            if (root->left) {
                nodes.push(root->left);
            }
            if (root->right) {
                nodes.push(root->right);
            }
        }
    }
}
```

## 由两种序列确定二叉树

有 `前序+中序` 和 `后序+中序` 两种组合，只有前序和后序序列是无法确定二叉树的。

理解前中后序序列顺序就很容易写出递归算法来构建二叉树了。

> 前序：根节点-左子树-右子树
> 中序：左子树-根节点-右子树
> 后序：左子树-右子树-根节点

### 根据前序和中序序列确定二叉树

首先根据前序序列找到根节点，即前序序列的第一个节点，然后在中序序列中找到根节点所在位置，这样就可以精确地获得左子树/右子树的 `前序+中序` 序列，递归地构建左右子树即可。

```cpp
TreeNode *buildTree0(std::unordered_map<int, int> &inorder_map, std::vector<int> &inorder, std::vector<int> &preorder, int in_start, int pre_start, int node_size) {
    /*
        inorder_map: 保存 inroder 的值的 index 映射，这里假定节点没有重复值
        inorder: 二叉树中序序列
        preorder: 二叉树前序序列
        in_start: 当前子树对应的中序序列在整个树的中序序列中的起始位置
        pre_start: 当前子树对应的前序序列在整个树的前序序列中的起始位置
        node_size: 当前子树的节点个数
    */
    TreeNode *root = nullptr;
    if (node_size) {
        root = new TreeNode(preorder[pre_start]);
        int root_index = inorder_map[root->val];
        int left_len = root_index - in_start;
        root->left = buildTree0(inorder_map, inorder, preorder, in_start, pre_start + 1, left_len);
        root->right = buildTree0(inorder_map, inorder, preorder, in_start + left_len + 1, pre_start + 1 + left_len, node_size - 1 - left_len);
    }
    return root;
}
```

### 根据前序和中序序列确定二叉树

与 `前序+中序` 类似。

```cpp
TreeNode *buildTree1(std::unordered_map<int, int> &inorder_map, std::vector<int>& inorder, std::vector<int>& postorder, int in_start, int post_start, int node_size) {
    // 参数含义与 `前序+中序` 类似
    TreeNode *root = nullptr;
    if (node_size) {
        root = new TreeNode(postorder[post_start + node_size - 1]);
        int root_index = inorder_map[root->val];
        int left_len = root_index - in_start;
        root->left = buildTree1(inorder_map, inorder, postorder, in_start, post_start, left_len);
        root->right = buildTree1(inorder_map, inorder, postorder, root_index + 1, post_start + left_len, node_size - 1 - left_len);
    }
    return root;
}
```
