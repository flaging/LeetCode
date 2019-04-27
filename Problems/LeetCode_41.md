# [LeetCode] 41. First Missing Positive

## 题目描述

Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

```C++
Input: [1,2,0]
Output: 3
```

**Example 2:**

```C++
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```C++
Input: [7,8,9,11,12]
Output: 1
```

**Note:**

Your algorithm should run in O(n) time and uses constant extra space.

## 要点

* 哈希表

## 方法：哈希表

由于需要对所有元素依次进行排序，所以使用哈希表的方式进行排序可以实现要求并且能够满足O(n)的时间复杂度。但本文中要求使用一定的空间，所以无法另外建立哈希表，只能在原有数组的基础上进行建立。

### 1.建立哈希表

通过一次遍历将所有1-n的数字构建哈希表，这里使用额外一个int型的空间。

### 2.寻找最小的缺失值

通过循环，一一比对当前位置与其值的关系。

### 3.代码

```C++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        //建立哈希表
        for(int i = 0; i < nums.size(); i++){
            //这里使用while保证所有替换到i地址的元素都会转移到合适的位置。
            while((nums[i] > 0) && (nums[i] <= nums.size()) && (nums[nums[i] - 1] != nums[i])){
                swap(nums[i], nums[nums[i] - 1]);
            }
        }
        //寻找最小缺失值
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] != i+1){
                return i + 1;
            }
        }
        return nums.size() + 1;
    }
};
```

### 4.结果

**Runtime:**

4 ms, faster than 100.00% of C++ online submissions for First Missing Positive.

**Memory Usage:**

8.6 MB, less than 100.00% of C++ online submissions for First Missing Positive.
