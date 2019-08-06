# [LeetCode] 412. Fizz Buzz

## 题目描述

Easy

Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

Example:

```C++
n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

Accepted

221,716

Submissions

370,258

## 要点

* 遍历
* 哈夫曼树

## 方法1

### 1.代码

```C++
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> vec;
        for(int i =1;i <= n; i++){
            if(!(i % 3)){
                if((i % 5)){
                    vec.push_back("Fizz");
                }else{
                    vec.push_back("FizzBuzz");
                }
            }else{
                if(!(i % 5)){
                    vec.push_back("Buzz");
                }else{
                    vec.push_back(to_string(i));
                }
            }
        }
        return vec;
    }
};
```

### 2.结果

**Success**

Details:

**Runtime:** 

8 ms, faster than 62.53% of C++ online submissions for Fizz Buzz.

**Memory Usage:** 

10.6 MB, less than 29.36% of C++ online submissions for Fizz Buzz.

## 方法2

### 1.代码
```C++
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> vec;
        for(int i =1;i <= n; i++){
            string str = "";
            if(!(i % 3))str += "Fizz";
            if(!(i % 5))str += "Buzz";
            vec.push_back(str == ""?to_string(i):str);
        }
        return vec;
    }
};
```

### 2.结果

**Success**

**Runtime:** 

4 ms, faster than 97.16% of C++ online submissions for Fizz Buzz.

**Memory Usage:** 

10.5 MB, less than 50.79% of C++ online submissions for Fizz Buzz.