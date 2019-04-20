# [LeetCode] 21. Merge Two Sorted Lists

## 题目描述

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```C++
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

## 要点

* 两个队列队首比较
* 边界条件处理

## 方法

### 1.建立头结点

新建一个节点，将其作为头结点。

### 2.队首元素比较

使用两个队队首是否存在作为循环条件，选取小的元素作为合并后队列的队尾。然后将队列前移，直至有一个队不存在队首。

### 3.处理最后一个元素

使用三目运算符将最后一个元素加入队列。

### 4.代码

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        //建立队首
        ListNode *res = new ListNode(INT_MIN);
        //建立队尾指针
        ListNode *temp = res;
        //当两个队列都有队首时，比较并排序
        while(l1 && l2){
            if( l1 -> val <= l2 -> val){
                temp -> next = l1;
                l1 = l1 -> next;
            }else{
                temp -> next = l2;
                l2 = l2 -> next;
            }
            temp = temp -> next;
        }
        //处理最后一个元素
        temp -> next = l1?l1:l2;
        return res -> next;
    }
};
```

### 5.结果

**Runtime:**

8 ms, faster than 100.00% of C++ online submissions for Merge Two Sorted Lists.

**Memory Usage:**

8.9 MB, less than 100.00% of C++ online submissions for Merge Two Sorted Lists.
