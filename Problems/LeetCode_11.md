# [LeetCode] 11. Container With Most Water

## 题目描述

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

**Example:**

```C++
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

## 要点

* 使用左右指针来指代迭代过程
* 寻找跳转更新的条件

## 方法

### 1.划分子问题

使用左右指针(或者变量)来定义当前状态的评价结果，然后计算最大值。

### 2.问题更新

当两位置的距离变短时，只有获得更高的宽的长度才**有可能**获得更大的值。只有该指针成为短板时才会向后/前寻找下一个更大的值。

### 3.终止条件

左指针大于右指针。

### 4.代码

```C++
class Solution {
public:
    int maxArea(vector<int>& height) {
        //最大面积计算
        int water = 0;
        //左右指针
        int left = 0, right = height.size() - 1;
        while(left < right){
            //获取桶的宽
            int high = min(height[left], height[right]);
            //计算最大面积
            water = max(water, high*(right - left));
            //移动左右指针
            //这里，只有该指针成为短板时才会向后/前寻找下一个更大的值
            while(height[left] <= high && left < right)left++;
            while(height[right] <= high && left < right)right--;
        }
        return water;
    }
};
