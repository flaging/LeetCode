# [LeetCode] 4. Median of Two Sorted Arrays

## 题目描述

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.

**Example 1:**

```C++
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```C++
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

## 要点

折半查找，两数组。

## 方法

方法原分析见本题讨论区(见[这里](https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/2481/Share-my-O(log(min(mn))-solution-with-explanation)))。

### 1.多数组的中值定义

对于单个数组来说，其中值的定义可以分为两种。

> 数组元素为单数时  
>$A[0],...A[i-1],A[i],A[i+1],....A[2i]$中，A[i]为中值；   
> 数组元素为复数时  
>$A[0],...A[i-1],A[i],A[i+1],....A[2i-1]$ 中，$(A[i-1] + A[i])/2$为中值。

而对于多个数组时，需要将其划分为两类$A^1[i]$和$A^2[j]$，当$A^1[i]$中的所有元素均小于$A^2[j]$时，那么中值为
$$median = \begin{cases}  
\frac{max(A^1[i]) + min(A^2[j])}{2} & (i+j)\%2 =1 \\
max(A^1[i]) & (i+j)\%2 =0 \& i >j \\ min(A^2[i]) & (i+j)\%2 =0 \& i<j
\end{cases}$$

### 2.对多数组划分

使用二分法对数组进行划分，这里假设第一个数组$A_1[i]$为$m$个，第二个数组$A_2[j]$的元素为$n$个且有$m<n$，那么会有$0<i<m$。这里，取$i$为$A_1[i]$的划分标准，$j$为$A_2[j]$的划分标准。而且，由于两组要满足均等划分的条件，所有当$m+n$为偶数时，要满足
$$i+j = (m+n)/2$$
所有当$m+n$为奇数时，要满足
$$i+j = (m+n+1)/2$$
那么可以根据以上公式只使用一个变量$i$就能完成数组的划分。这里使用整型数据的特性统一进行划分。

```C++
j = (m+n+1)/2-i;
```

### 3.划分后的可能情况

对于通常情形，每当确定一个$i$，就会出现一种划分方法。对于划分中的四个元素，那么会有如下三种情形。

* 情形1：$A_1[i-1]<A_2[j] \&\& A_2[j-1]<A_2[j]$
* 情形2：$A_1[i-1]>A_2[j] \&\& A_2[j-1]<A_2[j]$
* 情形3：$A_1[i-1]<A_2[j] \&\& A_2[j-1]>A_2[j]$

情形1说明已经正确划分了；情形2说明$i$取的太大了，需要减小；情形3说明$i$需要继续增大。增大减小过程使用的是上述的二分法。

最后，还要对两种特殊情形进行处理。

* 特殊情形1：$A_1[m-1]<A_2[0]$
* 特殊情形2：$A_1[0]>A_2[n-1]$

### 3.代码

```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        //进行处理，确保m<n
        int m = nums1.size() > nums2.size() ? nums2.size() : nums1.size();
        int n = nums1.size() > nums2.size() ? nums1.size() : nums2.size();
        
        vector<int>& num1 = nums1.size() > nums2.size() ? nums2 : nums1;
        vector<int>& num2 = nums1.size() > nums2.size() ? nums1 : nums2;
        
        //二分法
        int imin = 0;
        int imax = m;
        int i, j;
        while(imin <= imax){
            //根据二分法确定i,j
            i = (imin + imax)/2;
            j = (m + n + 1)/2 - i;
            
            //情形2
            if(i > 0 && num1[i - 1] > num2[j]) imax = i - 1;
            //情形3
            else if(i < m && num2[j - 1] > num1[i]) imin = i + 1;
            //情形1
            else{
                int max_num = 0, min_num = 0;
                //特殊情形2求前半部分最大值
                if(i == 0) max_num = num2[j - 1];
                //特殊情形3求前半部分最大值
                else if(j == 0) max_num = num1[i - 1];
                //通常情形
                else max_num = max(num1[i - 1], num2[j - 1]);
                
                //m+n为奇数
                if((m + n)%2 == 1) return max_num;
                
                //特殊情形1求后半部分最小值
                if(i == m) min_num = num2[j];
                //特殊情形2求后半部分最小值
                else if(j == n) min_num = num1[i];
                //通常情形
                else min_num = min(num1[i], num2[j]);
                
                //m+n为偶数
                return (min_num + max_num)/2.0;
            }
        }
        return 0;
    }
};
```

### 4.结果

**Runtime:**

16 ms, faster than 96.48% of C++ online submissions for Median of Two Sorted Arrays.

**Memory Usage:**

9.6 MB, less than 78.51% of C++ online submissions for Median of Two Sorted Arrays.
