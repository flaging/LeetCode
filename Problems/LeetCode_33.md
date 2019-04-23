# [LeetCode] 33. Search in Rotated Sorted Array

## 题目描述

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

```C++
(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).
```

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

**Example 1:**

```C++
Input: nums = [4,5,6,7,0,1,2], target = 0

Output: 4
```

**Example 2:**

```C++
Input: nums = [4,5,6,7,0,1,2], target = 3

Output: -1
```

## 要点

* 二分查找

由于时间复杂度为O(log n)，所以只能使用二分查找或者类似时间复杂度的算法。

## 方法

### 1.寻找翻转点

使用二分查找寻找数组当中的最小值，即翻转点。

### 2.二分查找搜索

使用二分查找寻找目标值。使用x%n进行新溢出后的值的重新定位。

### 3.代码

```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        //二分查找最小值
        int left = 0, right = n - 1;
        while(left < right){
            int mid = (right + left)/2;//left + (right - left)/2;
            if(nums[mid] > nums[right]){
                left = mid + 1;
            }else{
                right = mid;
            }
        }
        //保存最小值
        int minNums = left;
        left = 0, right = n - 1;
        //二分查找进行搜索
        while(left <= right){
            int mid = (right + left)/2;//left + (right - left)/2;
            //对溢出后的址进行重新定位
            int realMid = (minNums + mid)%n;
            if(nums[realMid] == target)return realMid;
            if(nums[realMid] < target){
                left = mid + 1;
            }else{
                right = mid -1;
            }
        }
        return -1;
    }
};
```

### 4.结果

**Runtime:**

8 ms, faster than 73.86% of C++ online submissions for Search in Rotated Sorted Array.

**Memory Usage:**

8.7 MB, less than 99.29% of C++ online submissions for Search in Rotated Sorted Array.
