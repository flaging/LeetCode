# [LeetCode] 17. Letter Combinations of a Phone Number

## 题目描述

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example:**

```C++
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:

Although the above answer is in lexicographical order, your answer could be in any order you want.
```

## 要点

* 输入的字符数是不一定的，所以不能简单使用循环嵌套

## 方法

## 1.预处理

* 处理数字串中没有数字的情况
* 将数字与字母的对应关系使用向量确定下来
* 返回值res初始化：若res为空，那么baisc一直为零，后面的循环不成立，所有res为有一个空字符串的向量。

### 2.循环遍历所有数字

* 使用一层循环遍历所有数字
* 通过map映射得到当前字符

### 3.两层循环生成当前字符下的所有可能

* 当前字符下的递归表示为every res[i-1] +  every map[num][k]，使用新变量保存这一结果
* 将结果覆盖给res
* 进行下一轮的计算

### 4.代码

```C++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> res;
        //特殊情况处理
        if(digits.empty())return {};
        //生成映射
        vector<string> map = {"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        //初始化res，保证后续basic初始值为1
        res.push_back("");
        //本层循环遍历所有数字
        for(int i = 0; i < digits.size(); i++){
            //计算字符对应的数字
            int num = digits[i] - '0';
            if(num > 9 || num < 0)break;
            //计算两层循环的边界len和basic
            int len = map[num].size();
            if(len < 1)continue;
            int basic = res.size();
            //计算当前结果，并用temp暂存
            vector<string> temp;
            for(int j = 0; j < basic; j++){
                for(int k = 0; k < len; k++){
                    //将新产生的字符串入栈
                    temp.push_back(res[j] + map[num][k]);
                }
            }
            //将结果覆盖给res
            res = temp;
        }
        return res;
    }
};
```

### 5.结果
**Runtime:**

 4 ms, faster than 100.00% of C++ online submissions for Letter Combinations of a Phone Number.
 
**Memory Usage:**

 8.8 MB, less than 45.83% of C++ online submissions for Letter Combinations of a Phone Number.
