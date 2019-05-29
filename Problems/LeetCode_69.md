# [LeetCode] 69. Sqrt(x)

## 题目描述

Implement int sqrt(int x).

Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

**Example 1:**

```C++
Input: 4
Output: 2
```

**Example 2:**

```C++
Input: 8
Output: 2
```

**Explanation:** The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.

## 要点

查找算法，二叉查找。

## 方法1

### 1.判断特殊情形

被开方数小于2的时候可以直接返回原值。

### 2.折半查找

通过折半查找找到小于该被开方数的数字。

### 3.向上查找

查找第一个满足被开方数在两个之间的数字。

### 4.代码

```C++
class Solution {
public:
    int mySqrt(int x) {
        //特殊情况处理
        if(x<2)return x;
        //折半查找
        int t = x/2;
        while(t>x/t){
            t=t/2;
        }
        //向上寻找
        while((t+1)<=x/(t+1)){
            t++;
        }
        return t;
    }
};
```

**注：这里使用$t*t>x$的判定方法会引起溢出，故采用$t>x/t$的方法。**

### 5.结果

**Runtime:**

28 ms, faster than 14.03% of C++ online submissions for Sqrt(x).

**Memory Usage:**

8.4 MB, less than 55.26% of C++ online submissions for Sqrt(x).

## 方法二：二叉查找

### 1.特殊情况处理

小于2的情况。

### 2.二叉查找

通过设计左右指针，不断二分查找得到最后值。

### 3.代码

```C++
class Solution {
public:
    int mySqrt(int x) {
        //特殊情况处理
        if(x<2) return x;
        //二叉查找
        int left = 0, right = x;
        while(left < right - 1){
            int mid =  left + (right - left)/2;
            if(mid > x/mid){
                right = mid;
            }else{
                left = mid;
            }
        }
        return left;
    }
};
```

### 4.结果

**Runtime:**

4 ms, faster than 95.44% of C++ online submissions for Sqrt(x).

**Memory Usage:**

8.2 MB, less than 72.00% of C++ online submissions for Sqrt(x).
