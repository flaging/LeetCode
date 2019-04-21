# [LeetCode] 26. Remove Duplicates from Sorted Array

## 题目描述

Given a sorted array nums, remove the duplicates *in-place* such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array** in-place with O(1) extra memory.

**Example 1:**

```C++
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

Given nums = [0,0,1,1,1,2,2,3,3,4],

```C++
Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```C++
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

## 要点

* 去重
* 输出非重复的元素

## 方法：左右指针遍历

### 1.处理特殊情况

处理数组为空的情况。

### 2.建立左右指针

* 当左右指针的值相同：右移右指针
* 当左右指针的值不同：将右指针的值移到左指针的下一个元素，燃油右移右指针

上述过程运行至右指针指向数组末尾，同时过程中使用左指针计数。

### 3.代码

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        //处理特殊情况
        if(nums.empty())return 0;
        //左右指针
        int left = 0,right =0;
        //遍历
        for(right = 0;right < nums.size(); right++){
            //左右值不同的情形
            if(nums[left] != nums[right]){
                nums[++left] = nums[right];
            }
        }
        return left + 1;
    }
};
```

### 4.结果

**Runtime:**

 24 ms, faster than 98.87% of C++ online submissions for Remove Duplicates from Sorted Array.

**Memory Usage:**

 9.9 MB, less than 99.45% of C++ online submissions for Remove Duplicates from Sorted Array.

## 方法二：记录重复数

### 1.建立标志位

用于记录重复数字的个数的变量$count$。

### 2.遍历

直接使用当前位置已经当前重复字符个数直接可以计算得该元素应该处于的位置为$i-count$。

### 3.代码

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        //记录重复个数
        int count = 0;
        for(int i = 1; i < nums.size(); i++){
            //记录重复个数
            if(nums[i] == nums[i-1])count++;
            //移动元素
            else{
                nums[i - count] = nums[i];
            }
        }
        return nums.size() - count;
    }
};
```

### 4.结果

**Runtime:**

 24 ms, faster than 98.87% of C++ online submissions for Remove Duplicates from Sorted Array.

**Memory Usage:**

 9.9 MB, less than 99.45% of C++ online submissions for Remove Duplicates from Sorted Array.
