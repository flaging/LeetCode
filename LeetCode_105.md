# [LeetCode] 105. Construct Binary Tree from Preorder and Inorder Traversal

## 题目描述

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note**:
You may assume that duplicates do not exist in the tree.

For example, given

```C++
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```C++
    3
   / \
  9  20
    /  \
   15   7
```

## 要点

经典题目，使用先序和中序遍历建立二叉树。

## 方法一：递归算法

根据先序遍历的起始位置将中序遍历分为两部分，左边部分位于左子树，右边部分位于右子树，左右子树分别递归调用该算法。

### 1.递归标准

树的建立来自于子树的建立，左右子树分别建立完成后，当前节点的子树就建立完成。

### 2.结束条件

当子树的根节点中的元素数量为零时，其左右子树均为NULL。

### 3.特殊处理

使用循环查找子树根节点在中序遍历中的位置，然后进行左右子树的构建。

### 4.代码

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return help(0, 0, inorder.size()-1, preorder, inorder);
    }
    TreeNode* help(int preStart, int inStart, int inEnd, vector<int>& preorder, vector<int>& inorder){
        //结束条件处理
        if(preStart > preorder.size()-1 || inStart > inEnd) return NULL; 
        TreeNode* root = new TreeNode(preorder[preStart]);
        //寻找子树根节点的位置
        int index;
        for(index = inStart; index <= inEnd; index++){
            if(inorder[index] == preorder[preStart]) break;
        }
        //建立左子树
        root->left = help(preStart + 1, inStart, index - 1, preorder,inorder);
        //建立右子树
        root->right = help(preStart + index -inStart +1, index +1, inEnd, preorder, inorder);
        //返回子树根节点
        return root;
    }
};
```

## 方法二：递归算法+哈希表

本方法是对方法一的改进。

### 1.哈希表的使用

使用哈希表对查找中序遍历的循环进行改进，内存使用量增多，但速度加快。

### 2.代码

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        //构建哈希表
        map<int,int> indexMap;
        for(int i = 0;i < inorder.size(); i++){
            indexMap[inorder[i]] = i;
        }
        return help(0, 0, inorder.size()-1, preorder, inorder,indexMap);
    }
    TreeNode* help(int preStart, int inStart, int inEnd, vector<int>& preorder, vector<int>& inorder,map<int,int>& indexMap){
        //结束条件处理
        if(preStart > preorder.size()-1 || inStart > inEnd) return NULL;
        TreeNode* root = new TreeNode(preorder[preStart]);
        //寻找子树根节点的位置
        int index = indexMap[preorder[preStart]];
        //建立左子树
        root->left = help(preStart + 1, inStart, index - 1, preorder,inorder, indexMap);
        //建立右子树
        root->right = help(preStart + index -inStart +1, index +1, inEnd, preorder, inorder, indexMap);
        //返回子树根节点
        return root;
    }
};
```

## 方法三：堆栈方法

