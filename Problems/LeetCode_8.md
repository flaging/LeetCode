# [LeetCode] 8. String to Integer (atoi)

## 题目描述

Implement atoi which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

**Note:**

Only the space character ' ' is considered as whitespace character.
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.

```C++
Example 1:
Input: "42"
Output: 42

Example 2:
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
Then take as many numerical digits as possible, which gets 42.

Example 3:
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.

Example 4:
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical digit or a +/- sign. Therefore no valid conversion could be performed.

Example 5:
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.Thefore INT_MIN (−231) is returned.
```

## 要点

* 编程思路要清晰
* 各部分要采用顺序结构，嵌套结构容易出问题

## 方法

### 1.空格字符

使用循环将空格字符跳过。

### 2.正负号

使用if-else结构将正负号进行解析，要注意，这里只能判断一次，故"+-2"这种的返回值是0。

### 3.非数字处理

当碰到一个非数字后，便将之前内容返回。

### 4.数字溢出处理

先对sum数据进行判断，然后再进行加和等处理。

另外，要注意$sum*10 + c - '0'$也可能会导致加的过程中溢出。

### 5.代码

```C++
class Solution {
public:
    int myAtoi(string str) {
        //正负号标志位、数字和、当前字符指向
        int f = 1;
        int sum =0;
        int i = 0;
        //空格去除
        while(i<str.length() && str[i] == ' ')i++;
        //正负数判断，并在判断后字符指向后移。
        if(i < str.length() && str[i] == '+'){
            f = 1;i++;
        }else if(i < str.length() && str[i] == '-'){
            f = -1;i++;
        }
        //对数字进行处理，当出现非数字时循环中断
        while(i<str.length() && str[i] >= '0' && str[i] <= '9'){
            //判断下一步是否溢出
            if((f ==1)&&(sum > 214748364 || sum ==214748364 && str[i]-'0'>=8)) return INT_MAX;
            if((f ==-1)&&(sum > 214748364 || sum ==214748364 && str[i]-'0'>=8)) return INT_MIN;
            //求和操作
            sum = str[i] - '0' + sum * 10;
            i++;
            
        }
        return f*sum;
    }
};
```
