# [LeetCode] 141. Linked List Cycle

## 题目描述

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

**Example 1:**

Input: head = [3,2,0,-4], pos = 1

Output: true

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

Explanation: There is a cycle in the linked list, where tail connects to the second node.


**Example 2:**

Input: head = [1,2], pos = 0

Output: true

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

Explanation: There is a cycle in the linked list, where tail connects to the first node.


Example 3:

Input: head = [1], pos = -1

Output: false

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

Explanation: There is no cycle in the linked list.
 

**Follow up:**

Can you solve it using O(1) (i.e. constant) memory?

## 要点

* 链表
* 快慢链表法


## 方法一：快慢链表法

### 1.建立两指针

一个作为快指针，一个作为慢指针。

### 2.指针前进

若存在闭环，那么慢指针总能追上快指针，若没有闭环，那么在快指针达到终点时循环结束，且快指针与慢指针不相同。

### 3.代码

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
    bool hasCycle(ListNode *head) {
        if(head == NULL || head -> next == NULL)return false;
        ListNode *fast = head -> next, * slow = head;
        while(fast != NULL && fast -> next != NULL && fast != slow){
            fast = fast -> next -> next;
            slow = slow -> next;
        }
        return fast ==slow;
    }
};
```

### 4.结果


**Runtime:** 

12 ms, faster than 85.65% of C++ online submissions for Linked List Cycle.

**Memory Usage:** 

9.9 MB, less than 20.34% of C++ online submissions for Linked List Cycle.

## 方法二：向量法

### 1.建立向量

用于记录当前指针。

### 2.进行循环

若有相同指针，那么说明有闭环。

### 3.代码

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
    bool hasCycle(ListNode *head) {
        vector<ListNode *> nodeSeen;
        while(head != NULL){
            if(nodeSeen.end() != find(nodeSeen.begin(), nodeSeen.end(), head))return true;
            else nodeSeen.push_back(head);
            head = head -> next;
        }
        return false;
    }
};
```

### 4.结果

**Runtime:** 

112 ms, faster than 9.75% of C++ online submissions for Linked List Cycle.

**Memory Usage:** 

10.2 MB, less than 15.23% of C++ online submissions for Linked List Cycle.
