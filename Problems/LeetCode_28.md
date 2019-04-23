# [LeetCode] 28. Implement strStr()

## 题目描述

Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

**Example 1:**

```C++
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```C++
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**Clarification:**

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when *needle* is an empty string. This is consistent to C's strstr() and Java's indexOf().

## 要点

* 字符串匹配
* 边界条件

## 方法一：循环遍历

首先按照首字母与字符串进行匹配，匹配成功后再比较后面的元素的个数的比较。

> 注意：边界条件的设置

### 1.代码

```C++

class Solution {
public:
    int strStr(string haystack, string needle) {
        //特殊情况的处理
        if(needle.empty())return 0;
        if(needle.size() > haystack.size())return -1;
        //外层为字符串的处理
        //这里i不需要进行到字符串最后，但=是必要的
        for(int i = 0; i <= haystack.size()-needle.size(); i++){
            int j = 0;
            //内层为第一个字符匹配后，进行后续字符的比较
            if(haystack[i+j] == needle[j]){
                for(; j < needle.size(); j++){
                    if(haystack[i+j] != needle[j]) break;
                }
                //判断是字符全部匹配，还是中间中断了
                if(j == needle.size()) return i;
            }
        }
        //匹配失败
        return -1;
    }
};
```

### 2.结果

**Runtime:**

 4 ms, faster than 100.00% of C++ online submissions for Implement strStr().

**Memory Usage:**

 9 MB, less than 99.22% of C++ online submissions for Implement strStr().

## 方法二：KMP方法

KMP方法是由Knuth-Morris-Pratt三人在1974年分别独立发明的一种字符串匹配的方法，具体介绍可见[维基百科](https://zh.wikipedia.org/zh-hans/%E5%85%8B%E5%8A%AA%E6%96%AF-%E8%8E%AB%E9%87%8C%E6%96%AF-%E6%99%AE%E6%8B%89%E7%89%B9%E7%AE%97%E6%B3%95)或者[百度百科](https://baike.baidu.com/item/KMP%E7%AE%97%E6%B3%95/10951804)。

