# [LeetCode] 3.Longest Substring Without Repeating Characters

## 题目描述

Given a string, find the length of the longest substring without repeating characters.

**Example 1:**

Input: "abcabcbb"

Output: 3

Explanation: The answer is "abc", with the length of 3. 

**Example 2:**

Input: "bbbbb"

Output: 1

Explanation: The answer is "b", with the length of 1.

**Example 3:**

Input: "pwwkew"

Output: 3

Explanation: The answer is "wke", with the length of 3.

**Note that the answer must be a substring, "pwke" is a subsequence and not a substring.**

## 要点

* 使用哈希表提高查找重复字母位置的速度。
* 维护一个没有重复字母的窗口。

## 方法

### 1.窗口创建

窗口有一个左右边界，分别以 $start$ 和 $i$ 来表示。

### 2.哈希表创建

创建一个有256个元素（ASCII码）的向量，记录字符 $s[i]$ 所在的位置 $i$。

### 3.重复元素处理

当窗口右边界引入一个新的字符的时候，可能会出现两种情况：

1.字符与当前窗口没有重复：

将该字符位置记录到哈希表中，将当前当前窗口的最大值 $i - start$ 与整个过程中的窗口最大值作比较并更新。

2.字符与当前窗口中其中一个重复：

添加字符前的窗口大小就是该窗口能够达到的最大大小。原窗口被重复的字符分成了两部分，使用哈希表可以查找到该重复字符的位置为 $dict[s[i]]$ ，后续将窗口的左端移到该重复字符处，建立新的窗口。

### 4.代码

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        vector<int> dict(256, -1);
        int maxLen =0, start = -1;
        for(int i = 0; i < s.size(); i++){
            if(dict[s[i]] > start){
                start = dict[s[i]];
            }
            dict[s[i]] = i;
            maxLen = max(maxLen, i - start);
        }
        return maxLen;
    }
};
```



