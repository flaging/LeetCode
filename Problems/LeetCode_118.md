# [LeetCode] 118. Pascal's Triangle

## 题目描述

Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

In Pascal's triangle, each number is the sum of the two numbers directly above it.

**Example:**

```C++
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

## 要点

难度：Easy。
循环，迭代。

## 方法：直接法

### 1.寻找求解公式

对于除边界条件的的元素$A(i,j)$，有
$$A(i,j) = A(i-1,j-1)+A(i-1,j)$$
边界条件为$j = 0$或者$j=i$。

### 2.迭代实现

使用双层循环实现以上迭代规则。

### 3.代码

```C++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> res;
        for(int i = 0; i < numRows; i++){
            vector<int> res1;
            for(int j = 0; j <= i; j++){
                if(j == 0 || j == i)res1.push_back(1);
                else{
                    res1.push_back(res[i-1][j-1]+res[i-1][j]);
                }
            }
            res.push_back(res1);
        }
        return res;
    }
};
```

### 4.结果

**Runtime:**

4 ms, faster than 85.60% of C++ online submissions for Pascal's Triangle.

**Memory Usage:**

9 MB, less than 10.98% of C++ online submissions for Pascal's Triangle.
