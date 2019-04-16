# [LeetCode] 20. Valid Parentheses

## 题目描述

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

* Open brackets must be closed by the same type of brackets.
* Open brackets must be closed in the correct order.

**Note that an empty string is also considered valid.**

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```

## 要点

* 使用栈结构进行处理

## 方法一：栈

### 1.建立栈结构

使用向量实现。

### 2.左括号入栈

将遍历过程中的左半部分入栈，等待比较。

### 3.对比出栈

将遇到的右括号将其栈顶的对应值出栈，抵消。

### 4.代码

```C++
class Solution {
public:
    bool isValid(string s) {
        //建立左右括号间的映射
        map<char,char> relat;
        relat[')'] = '(';
        relat['}'] = '{';
        relat[']'] = '[';
        //处理特殊情况
        if(s.size()<0|| s.size()%2 == 1) return false;
        vector<char> res;
        //遍历处理所有括号
        for(int i = 0; i < s.size(); i++){
            switch (s[i]){
                //左括号入栈
                case '(':
                case '{':
                case '[':
                    {res.push_back(s[i]);break;}
                //右括号对比出栈
                case ')':
                case '}':
                case ']':{
                    if(res.size() <=0 || res.back() != relat[s[i]]) return false;
                    else{
                        res.pop_back();
                    }
                    break;
                }
                //既不是左括号又不是右括号，那么说明混入杂质
                default:
                    return false;
            }
        }
        //当全部两两抵消，表明配对成功
        if(res.size() == 0){
            return true;
        }else{
            return false;
        }
    }
};
```

### 5.结果

**Runtime: **

8 ms, faster than 20.27% of C++ online submissions for Valid Parentheses.

**Memory Usage:**

8.3 MB, less than 100.00% of C++ online submissions for Valid Parentheses.

## 方法二：栈（2）

### 1.思路与方法一相同，但代码更简洁

### 2.代码

```C++
class Solution {
public:
    bool isValid(string s) {
        stack<char> res;
        for(char c : s){
            //处理左括号
            if(c == '(' || c == '{' || c == '['){
                res.push(c);
            }
            //处理右括号
            else{
                if(res.empty()) return false;
                if(c == '}' && res.top() != '{') return false;
                if(c == ')' && res.top() != '(') return false;
                if(c == ']' && res.top() != '[') return false;
                res.pop();
            }
        }
        return res.empty();
    }
};
```

### 3.结果

**Runtime:**

4 ms, faster than 100.00% of C++ online submissions for Valid Parentheses.

**Memory Usage:**

8.3 MB, less than 100.00% of C++ online submissions for Valid Parentheses.
