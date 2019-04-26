# [LeetCode] 34. Find First and Last Position of Element in Sorted Array

## 题目描述

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

**Example 1:**

```C++
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```C++
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

## 要点

* 二分法

## 方法：二分法

### 1.特殊情况处理

主要处理数组向量为空的情况。

### 2.二分法寻找左边界

使用二分法寻找左边界，注意判断条件为$nums[mid] < target$，这是由于需要寻找左边界，而不是等于的点。

### 3.特殊情况处理

处理数组向量中并没有该数字的情况。

### 4.二分法寻找右边界

使用二分法，在左边界到数组末尾之间寻找右边界。

### 5.代码

```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {

        vector<int> res(2,-1); 
        //处理特殊情况
        if(nums.empty())return res;
        //寻找左边界
        int i = 0, j = nums.size() - 1;
        while(i < j){
            int mid = (i + j)/2;
            //判断条件
            if(nums[mid] < target)i = mid + 1;
            else j = mid;
        }
        //特殊情况处理
        if(nums[i] != target)return res;
        res[0] = i;
        //在左边界右侧寻找右边界
        j = nums.size() - 1;
        while(i < j){
            //保证不会进入死循环
            int mid = (i + j+ 1)/2;
            //判断条件
            if(nums[mid] > target) j =mid - 1;
            else i =mid;
        }
        res[1] = j;
        return res;
    }
};
```

### 6.结果

**Runtime:**

12 ms, faster than 83.82% of C++ online submissions for Find First and Last Position of Element in Sorted Array.

**Memory Usage:**

10.5 MB, less than 98.92% of C++ online submissions for Find First and Last Position of Element in Sorted Array.
