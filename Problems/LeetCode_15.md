# [LeetCode] 15. 3Sum

## 题目介绍

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```C++
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

## 要点

* 将问题划分为子问题

## 方法一：三重循环

### 1.建立三层循环

使用三层循环对所有可能情况进行遍历

### 2.去掉重复值

使用自带函数对其进行去重。

### 3.代码

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int len = nums.size();
        sort(nums.begin(),nums.end());
        //排序，保证输出向量从小到大，便于后面去重
        vector<vector<int>> res;
        for(int i = 0; i < len; i++){
            for(int j = i+1;j < len; j++){
                for(int k = j+1; k < len; k++){
                    if(0 == nums[i] + nums[j] + nums[k]){
                        vector<int> temp;
                        temp.push_back(nums[i]);
                        temp.push_back(nums[j]);
                        temp.push_back(nums[k]);
                        res.push_back(temp);
                    }

                }
            }
        }
        //去除重复变量
        sort(res.begin(),res.end());
        res.erase(unique(res.begin(), res.end()), res.end());
        return res;
    }
};
```

## 方法二：两重循环

### 1.元素排序

便于后续元素查重。

### 2.循环遍历第一个元素

确定第一个元素，然后第二三个元素就变成了一个确定值的2sum问题了。

### 3.解决2sum问题

由于元素有序，可从最前和最后开始算起，计算两者的和。

另外，可以使用一个很小的循环跳过重复元素的比较，这里使用了三个while循环来分别去除三个元素的重复值。

### 4.代码

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        //特殊情况处理
        if(nums.size() < 3) return {};
        //排序以方便去重
        sort(nums.begin(), nums.end());
        //循环部分
        for(int i = 0; nums[i] <= 0 && i < nums.size() - 2; i++){
            //解决2sum子问题
            int left = i+1, right = nums.size() -1;
            while(left < right){
                int sum = nums[right] + nums[left];
                if(sum + nums[i] > 0){
                    right--;
                }else if(nums[i] + sum < 0){
                    left++;
                }else{
                    vector<int> temp = vector(3,0);
                    temp[0] = nums[i];
                    temp[1] = nums[left++];
                    temp[2] = nums[right--];
                    res.push_back(temp);
                    //对第二个和第三个元素去重
                    while(left < right && nums[left] == temp[1])left++;
                    while(left < right && nums[right] == temp[2])right--;
                    
                }

            }
            //对第一个元素去重
            while(i < nums.size() -2 && nums[i+1] == nums[i])i++;
        }
        return res;
    }
};
```
