# [LeetCode] 29. Divide Two Integers

## 题目描述

Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero.

**Example 1:**

Input: dividend = 10, divisor = 3

Output: 3

**Example 2:**

Input: dividend = 7, divisor = -3

Output: -2

**Note:**

* Both dividend and divisor will be 32-bit signed integers.
* The divisor will never be 0.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[−2^{31},  2^{31} − 1]$. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.

## 要点

* 除法运算
* 边界处理
* 位运算

## 方法：位运算

对于被除数$A$和除数$B$，他们满足如下公式

$$C=\frac{A}{B}-D$$

在忽略余数的前提下，可以将商写成二进制的形式

$$C=(a_02^0+a_12^1...+a_n2^n)=\frac{A}{B}$$

或者

$$a_02^0B+a_12^1B+...+a_n2^nB = A$$

则

$$a_i2^iB \leq A - \sum_{j = i+1}^n a_j2^jB$$

### 1.处理溢出异常

题目中要求当溢出时输出最大值，出现的可能是$-2^{31}/-1$。

### 2.循环终止条件

当被除数剩余部分小于除数，则不再寻找另外的值。

### 3.计算$a_i$

原理根据上文最后一式。

### 4.代码

代码来源于LeetCode讨论区的[这里](https://leetcode.com/problems/divide-two-integers/discuss/13407/C%2B%2B-bit-manipulations)。

```C++
class Solution {
public:
    int divide(int dividend, int divisor) {
        \\处理溢出的情况
        if (dividend == INT_MIN && divisor == -1) {
            return INT_MAX;
        }
        \\转换格式，防止溢出
        long dvd = labs(dividend), dvs = labs(divisor), ans = 0;
        //计算正负号
        int sign = dividend > 0 ^ divisor > 0 ? -1 : 1;
        //计算a_0....a_n
        while (dvd >= dvs) {
            long temp = dvs, m = 1;
            //计算a_i
            while (temp << 1 <= dvd) {
                temp <<= 1;
                m <<= 1;
            }
            dvd -= temp;
            ans += m;
        }
        return sign * ans;
    }
};
```

### 5.结果

**Runtime:**

 4 ms, faster than 100.00% of C++ online submissions for Divide Two Integers.
 
**Memory Usage:**

 8.2 MB, less than 99.69% of C++ online submissions for Divide Two Integers.
