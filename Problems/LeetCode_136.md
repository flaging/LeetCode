# [LeetCode] 136. Single Number

## 题目描述

Given a non-empty array of integers, every element appears twice except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,1]
Output: 1
```

**Example 2:**

```
Input: [4,1,2,1,2]
Output: 4
```

## 要点

* 位运算
* 重复元素
* 哈希表


## 方法一：位运算方法

### 1.位运算特性

位运算存在异或运算，具有类似于正负抵消的效应：即一个元素异或两次，对原来的元素没有其他改变。


### 2.对所有元素进行位运算

由于只有一个奇数次的元素，所以最后所剩的就是它。

### 3.代码

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int sum = 0;
        for(int i = 0; i < nums.size(); i++){
            sum ^= nums[i];
        }
        return sum;
    }
};
```

### 4.结果

**Runtime:** 16 ms, faster than 81.78% of C++ online submissions for Single Number.
**Memory Usage:** 9.5 MB, less than 89.78% of C++ online submissions for Single Number.

## 方法二：哈希表方法

### 1.方法

使用哈希表查询的方式查看元素是否在表内。如果元素在表内，那么说明有两个元素相同，那么将其移除表。如果表内查询不到那么说明目前为止没有该元素或者该元素数量为偶数个了，将其加入到哈希表中。

### 2.代码

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        map<int, int> hashmap;
        hashmap[nums[0]] = 0;
        for(int i =1; i < nums.size(); i++){
            if(hashmap.find(nums[i]) != hashmap.end()){
                hashmap.erase(nums[i]);
            }else{
                hashmap[nums[i]] = i;
            }
        }
        int a = 0;
        return hashmap.begin()->first;
    }
};
```

### 3.结果

**Runtime:** 

32 ms, faster than 10.63% of C++ online submissions for Single Number.

**Memory Usage:** 

11.7 MB, less than 5.04% of C++ online submissions for Single Number.

## 方法三：哈希表方法的set实现

### 1.方法

同方法二，实现方法由map转为set。

### 2.代码

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_set<int> mySet;
    for(int i = 0;i < nums.size();++i){
        if(mySet.find(nums[i]) == mySet.end()) mySet.insert(nums[i]);
        else mySet.erase(nums[i]);
    }
    auto it = mySet.begin();
    return *it;
    }
};
```

### 3.结果

**Runtime:** 

28 ms, faster than 15.13% of C++ online submissions for Single Number.

**Memory Usage:** 

11.5 MB, less than 14.09% of C++ online submissions for Single Number.
