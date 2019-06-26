# [LeetCode] 101. Symmetric Tree

## 题目描述

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
 

But the following [1,2,2,null,3,null,3] is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

Difficulty: **easy**
## 要点

* 二叉树遍历
* 先序遍历
* 后序遍历
* 队列

## 方法一：队列法

### 1.建立队列

用于存储先序和后序队列的访问顺序。

### 2.进行判断

比较过程中使用两个指针分别指向镜像位置，那么两个指针共有四种可能情况：

* 左右皆为空：符合镜像要求
* 左空右不空：不符合镜像要求
* 左不空右空：不符合镜像要求
* 左右皆不空：根据左右的实际值确定是否符合要求

### 3.代码

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
    bool isSymmetric(TreeNode* root) {
        //处理特殊情况
        if(root == NULL)return true;
        //建立队列
        queue <TreeNode *> QueLeft,QueRight;
        //左右指针指向镜像位置
        TreeNode *left, *right;
        QueLeft.push(root -> left);
        QueRight.push(root -> right);
        //循环结束条件，所有元素遍历完成
        while(!QueLeft.empty() && !QueRight.empty()){
            left = QueLeft.front();
            QueLeft.pop();
            right = QueRight.front();
            QueRight.pop();
            //四种情况进行判断
            if((NULL == left) && (NULL == right))continue;
            if((NULL == left) || (NULL == right))return false;
            if(left -> val != right -> val) return false;
            //先序遍历
            QueLeft.push(left -> left);
            QueLeft.push(left -> right);
            //后续遍历
            QueRight.push(right -> right);
            QueRight.push(right -> left);
        }
        return true;
    }
};
```


### 4.结果

**Runtime:** 

8 ms, faster than 74.73% of C++ online submissions for Symmetric Tree.

**Memory Usage:** 

14.8 MB, less than 50.96% of C++ online submissions for Symmetric Tree.

## 方法二：队列改进法

将队列中的两个队列合并为一个队列即可。
空间复杂度几乎不会改变。

### 1.代码

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
    bool isSymmetric(TreeNode* root) {
        if(root == NULL)return true;
        queue <TreeNode *> Que;
        TreeNode *left, *right;
        Que.push(root -> left);
        Que.push(root -> right);
        while(!Que.empty()){
            left = Que.front();
            Que.pop();
            right = Que.front();
            Que.pop();
            if((NULL == left) && (NULL == right))continue;
            if((NULL == left) || (NULL == right))return false;
            if(left -> val != right -> val) return false;
            Que.push(left -> left);
            Que.push(right -> right);
            Que.push(left -> right);
            Que.push(right -> left);
        }
        return true;
    }
};
```

### 2.结果

**Runtime:** 

8 ms, faster than 74.73% of C++ online submissions for Symmetric Tree.

**Memory Usage:** 

14.8 MB, less than 53.10% of C++ online submissions for Symmetric Tree.

## 方法三：递归法

### 1.子树拆分

原问题可以转化为左右两棵子树的的比较，即**左子树的左部分于右子树的右部分相等，右子树的左部分于左子树的右部分相等**。

### 2.情况分析

与方法一中的四种情况相同。

### 3.代码

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
    //建立子函数
    bool isMirror(TreeNode *left, TreeNode *right){
        //处理三种情况
        //第一种情况说明本节点是对应的，所以返回true
        if(NULL == left && NULL == right)return true;
        if(NULL == left || NULL == right)return false;
        //判断下一级子树
        return (left -> val == right -> val) && isMirror(left -> right, right -> left) && isMirror(left -> left, right -> right);
    }
    bool isSymmetric(TreeNode* root) {
        return isMirror(root,root);
    }
};
```

### 4.结果

**Runtime:** 

4 ms, faster than 92.31% of C++ online submissions for Symmetric Tree.

**Memory Usage:** 

14.6 MB, less than 95.13% of C++ online submissions for Symmetric Tree.
