# [LeetCode] 53. Maximum Subarray

## 题目描述

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**

```C++
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

**Follow up:**

If you have figured out the O(n) solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

## 要点

子问题求解，分治法。

## 方法一：遍历

### 1.建立中间状态数组

数组大小与原数组相同，初始化值均为零。

### 2.遍历

遍历中有两层循环，第一层循环表示首先求1个元素求和的结果，然后求2个元素的求和结果......直至n个元素的求和结果。

第二层循环用于求解每个位置的求和结果。

最终得到第一个位置前一个元素的求和结果，前两个元素的求和结果......

由于第一层循环在更新的时候会将其所有内容均覆盖掉，故在第二层循环中就将各个数值的最大值提前计算出来。

这里相对于传统遍历的优势在于，每个元素的求和值使用一次。例如，在求1-3个元素的和的时候和求1-5的元素和的时候，1-3的元素和是直接拿过来求得1-4的元素和，然后再用1-4求和的结果求1-5的结果。劣势在于需要增加空间复杂度。

### 3.代码

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res_max = INT_MIN;
        int array[nums.size()] = {0};
        int k = 0;
        //第一层循环
        for(int i = nums.size(); i > 0; i--){
            //第二层循环
            for(int j = 0; j < i; j++){
                array[j] = array[j] + nums[j + k];
                res_max = max(res_max, array[j]);
            }
            k++;
        }
        return res_max;
    }
};
```

### 4.结果

**Runtime:**

832 ms, faster than 5.06% of C++ online submissions for Maximum Subarray.

**Memory Usage:**

9.4 MB, less than 61.82% of C++ online submissions for Maximum Subarray.

## 方法二：动态规划

本方法来源于本问题的讨论区(见[这里](https://leetcode.com/problems/maximum-subarray/discuss/20193/DP-solution-and-some-thoughts))。

### 1.动态规划子问题推导

动态规划中的原问题应该是在数组中寻找一个$A[i:j]$以保证其和最大。但在动态规划中多按照一个变量进行子问题划分，若多个变量那么子问题就很复杂。那么可以将子问题划分为如下情形：

*求以$A[i]$为结尾的子数组的和的最大值？*

### 2.子问题推导关系式

此时，子问题的关系可以表示为

$$dp[i] = A[i] + (dp[i-1]>0?dp[i-1]:0)$$

即，当以$i-1$个元素为结尾的子数组的和为负时，可以将之前部分舍弃而直接从当前的元素$A[i]$开始记录。

### 3.代码

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        //预处理
        int n = nums.size();
        int dp[n] = {nums[0]};
        int max_nums = nums[0];
        //动态规划计算
        for(int i = 1; i < n; i++){
            dp[i] = nums[i] + (dp[i - 1] > 0?dp[i - 1]:0);
            max_nums = max(max_nums, dp[i]);
        }
        return max_nums;
    }
};
```

### 4.结果

**Runtime:**

4 ms, faster than 99.78% of C++ online submissions for Maximum Subarray.

**Memory Usage:**

9.4 MB, less than 56.45% of C++ online submissions for Maximum Subarray.
