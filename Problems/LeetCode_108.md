# [LeetCode] 

## 题目描述

Given an array where elements are sorted in **ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

**Example:**

Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:
```
      0
     / \
   -3   9
   /   /
 -10  5
 ```

## 要点

* 二分法
* 二叉树构建
* 递归法

由于题目中提到该数组是**非递减**的，所以为了能够保证平衡的拆分方法就是将中位数所在的节点作为根节点，左右分别进行递归的划分。

## 方法一：递归构建

### 1.处理特殊情况

当数组元素为一个或者零个的时候，需要进行特殊处理。（在递归的子数组中也是需要这么做）

### 2.二分数组

将数组二分，左右子树分别递归。

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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        //特殊情况的处理
        if(nums.size() == 0)return NULL;
        if(nums.size() == 1)return new TreeNode(nums[0]);
        //二分法
        int mid = nums.size()/2;
        TreeNode * root = new TreeNode(nums[nums.size()/2]);
        //将左右子树元素分别保存
        vector<int> leftInts(nums.begin(), nums.begin() + nums.size()/2);
        vector<int> rightInts(nums.begin() + nums.size()/2 + 1, nums.end());
        //递归调用
        root -> left = sortedArrayToBST(leftInts);
        root -> right = sortedArrayToBST(rightInts);
        return root;
    }
};
```

### 4.结果

**Runtime:** 

24 ms, faster than 56.31% of C++ online submissions for Convert Sorted Array to Binary Search Tree.

**Memory Usage:** 

24.6 MB, less than 11.94% of C++ online submissions for Convert Sorted Array to Binary Search Tree.

## 方法二：递归构建改进

在方法一中出现多次声称向量以保存左右子树的元素的行为，极大的耗费了内存。方法二采用原数组加，左右子树范围的方法进行构建。由结果看出，在不进行多次内存复制的行为后，速度和内存使用都有了很大的提升。

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
    //重新构造函数，包含begin和end元素
    TreeNode* sortedArrayToBST2(vector<int>& nums, int begin, int end){
        //特殊情况处理
        if(begin > end)return NULL;
        if(begin == end)return new TreeNode(nums[begin]);
        //二分
        int mid = begin + (end + 1 - begin)/2;
        TreeNode* root = new TreeNode(nums[mid]);
        //递归调用
        root -> left = sortedArrayToBST2(nums, begin, mid - 1 );
        root -> right = sortedArrayToBST2(nums, mid + 1,end);
        return root;
    }
public:
    //实际求解方法调用重新构造的函数
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedArrayToBST2(nums, 0, nums.size()-1);
    }
};
```
### 2.结果

**Runtime:** 

20 ms, faster than 79.11% of C++ online submissions for Convert Sorted Array to Binary Search Tree.

**Memory Usage:** 

21 MB, less than 65.68% of C++ online submissions for Convert Sorted Array to Binary Search Tree.
