# [LeetCode] 70. Climbing Stairs

## 题目描述

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

**Example 1:**

```C++
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**

```C++
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

## 要点

* 递推关系推导
* 斐波那契数列求解
* 循环队列(取余运算)

## 方法

分析见程序员小灰灰之[漫画：什么是动态规划？(整合版)](http://mp.weixin.qq.com/s?__biz=MzIxMjE5MTE1Nw==&mid=2653190796&idx=1&sn=2bf42e5783f3efd03bfb0ecd3cbbc380&chksm=8c990856bbee8140055c3429f59c8f46dc05be20b859f00fe8168efe1e6a954fdc5cfc7246b0&scene=21#wechat_redirect)

### 1.建立初始队列

建立三元素的数组，初始条件为$F(1) = 1$, $F(2) = 2$。

### 2.递推

根据所得的递推公式$F(N) = F(N-1) + F(N-2)$进行运算。

### 3.代码

```C++
class Solution {
public:
    int climbStairs(int n) {
        //初始化
        int stair[3] = {1,2,0};
        //逐步求解
        for(int i = 2; i < n; i++){
            stair[i%3] = stair[(i - 1)%3] + stair[(i - 2)%3];
        }
        return stair[(n-1)%3];
    }
};
```

### 4.结果

**Runtime:**

0 ms, faster than 100.00% of C++ online submissions for Climbing Stairs.

**Memory Usage:**

8.2 MB, less than 79.04% of C++ online submissions for Climbing Stairs.
