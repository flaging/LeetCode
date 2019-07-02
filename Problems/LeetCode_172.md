# [LeetCode] 172. Factorial Trailing Zeroes

## 题目描述

Given an integer n, return the number of trailing zeroes in n!.

**Example 1:**

```
Input: 3
Output: 0

Explanation: 3! = 6, no trailing zero.
```

**Example 2:**

```
Input: 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```

**Note:** Your solution should be in logarithmic time complexity.


## 要点

* 数学特征分析
* 递归法

## 方法一：数学法

### 1.分析特征

对于一个数，有多少个零就是寻找其因式分解中有多少个$10$，而$10=2\times 5$。而在$1-n$中含有数字$5$的有$n/5$个，含有$2$的数量远大于含有$5$的数量，所以可以根据$5$的数量确定有多少个$0$。但是要注意，像$25$这种数字包含超过1个$5$。

### 2.代码

```C++
class Solution {
public:
    int trailingZeroes(int n) {
        int count = 0;
        while(n){
            count += n/5;
            n /= 5;
        }
        return count;
    }
};
```

### 3.结果

**Runtime:** 

8 ms, faster than 15.05% of C++ online submissions for Factorial Trailing Zeroes.

**Memory Usage:** 

8.2 MB, less than 34.35% of C++ online submissions for Factorial Trailing Zeroes.

## 方法二：数学法改进

### 1.改进内容

对于数学法中的程序，每次运行至除完剩数字1-4时还会进行一次运算，可以将这一运算省略掉。
### 2.代码

```C++
class Solution {
public:
    int trailingZeroes(int n) {
        int count = 0;
        while(n/5){
            count += n/5;
            n /= 5;
        }
        return count;
    }
};
```

### 3.结果
**Runtime:** 

0 ms, faster than 100.00% of C++ online submissions for Factorial Trailing Zeroes.

**Memory Usage:** 

8.2 MB, less than 39.41% of C++ online submissions for Factorial Trailing Zeroes.

## 方法三：递归法

### 1.递归法

对于像$25$这种含有多个$5$的数字，可以采用循环的方法进行处理，效率较高。也可以采用递归的方法，虽然效率不一定高，但程序和逻辑更加简洁。

### 2.代码

```C++
class Solution {
public:
    int trailingZeroes(int n){
    return n == 0 ? 0 : n / 5 + trailingZeroes(n / 5);
    }
};
```

### 3.结果

**Runtime:** 

4 ms, faster than 76.30% of C++ online submissions for Factorial Trailing Zeroes.

**Memory Usage:** 

8.3 MB, less than 19.18% of C++ online submissions for Factorial Trailing Zeroes.
