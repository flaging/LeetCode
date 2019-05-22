# [LeetCode] 46. Permutations

## 题目描述

Given a collection of distinct integers, return all possible permutations.

**Example:**

```C++
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

## 要点

* 向量操作
* 循环

## 方法：循环插入

### 1.获取元素

通过循环遍历数组，元素个数从$1-n$依次进行处理。

### 2.插入操作

对于获取的每个元素，可以在原有的数组基础上进行插入操作，插入位置共$i+1$个。

### 3.代码

```C++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        //建立向量
        vector<vector<int>> res={{}};
        //依次选取变量进行操作
        for(int i = 0; i < nums.size(); i++){
            vector<vector<int>> temp;
            //插入操作
            for(auto j : res){
                for(int k = 0; k < j.size() + 1; k++){
                    vector<int> temp2 = j;
                    temp2.insert(temp2.begin()+k,nums[i]);
                    temp.push_back(temp2);
                }
            }
            res = temp;
        }
        return res;
    }
};
```

### 4.结果

**Runtime:**

24 ms, faster than 8.69% of C++ online submissions for Permutations.

**Memory Usage:**

10 MB, less than 23.66% of C++ online submissions for Permutations.
