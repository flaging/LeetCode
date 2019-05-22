# [LeetCode] 38. Count and Say

## 题目描述

The count-and-say sequence is the sequence of integers with the first five terms as following:

```C++
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.

Given an integer n where 1 ≤ n ≤ 30, generate the nth term of the count-and-say sequence.

**Note**: Each term of the sequence of integers will be represented as a string.

**Example 1:**

```C++
Input: 1
Output: "1"
```

**Example 2:**

```C++
Input: 4
Output: "1211"
```

## 要点

字符串处理。
## 方法

### 1.特殊情况处理

处理n为不合理值的情况。

### 2.更新字符串

在遍历中获取相同字符串内容和其个数，并作为下一次字符串的内容。

### 3.代码

```C++
class Solution {
public:
    string countAndSay(int n) {
        //特殊情况处理
        if(n<=0)
            return "";
        int i = 1;
        string res = "1";
        //大循环与n有关
        while(i < n){
            int j = size(res);
            string temp = "";
            int k=0, m=0;
            //循环遍历相同字符串信息
            while(k < j){
                while(res[k]==res[m])k++;
                temp=temp+to_string(k-m)+res[m];
                m=k;
            }
            res=temp;
            i++;
        }
        return res;
    }
};
```

### 4.结果

**Runtime:**

24 ms, faster than 16.13% of C++ online submissions for Count and Say.

**Memory Usage:**

59 MB, less than 11.85% of C++ online submissions for Count and Say.
