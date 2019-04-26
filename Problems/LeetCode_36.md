# [LeetCode] 36. Valid Sudoku

## 题目描述

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

**Example 1:**

```C++
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

**Example 2:**

```C++
Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
```
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

**Note:**

* A Sudoku board (partially filled) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.
* The given board contain only digits 1-9 and the character '.'.
* The given board size is always 9x9.

## 要点

* 哈希表
* 遍历

## 方法：哈希表

### 1.建立哈希表

对每一行、列以及方块建立一个$1\times 9$的哈希表，存储是否当前是否出现过该元素。

### 2.转化方块的哈希表坐标

坐标值为$k =\lfloor i/3 \rfloor\times3+\lfloor j/3\rfloor$，其中$\lfloor i/3 \rfloor$为向下取整符号。

### 3.判断

如果在行、列、块中有一个元素出现过，返回false。若都没出现过，将对应的元素置1。

### 3.代码

```C++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        //初始化哈希表为0
        bool flag1[9][9] = {0}, flag2[9][9] = {0}, flag3[9][9] = {0};
        //遍历
        for(int i = 0; i < 9; i++){
            for(int j = 0; j < 9; j++){
                //处理未填写的情况
                if(board[i][j] == '.')continue;
                int num = board[i][j] - '0' - 1;
                int k = i/3 *3 + j/3;
                //判断是否出现过
                if(flag1[i][num] || flag2[j][num] || flag3[k][num]){return false;}
                else{
                    flag1[i][num] = 1;
                    flag2[j][num] = 1;
                    flag3[k][num] = 1;
                }
            }
        }
        return true;
    }
};
```


### 4.结果

**Runtime:**

16 ms, faster than 95.41% of C++ online submissions for Valid Sudoku.

**Memory Usage:**

9.5 MB, less than 98.82% of C++ online submissions for Valid Sudoku.
