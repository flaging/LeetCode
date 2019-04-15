# [LeetCode] 19. Remove Nth Node From End of List

## 题目描述

Given a linked list, remove the n-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given n will always be valid.

**Follow up:**

Could you do this in one pass?

## 要点

* 单向链表：只能从前往后访问
* 删除倒数第n个元素：要首先访问到最后一个元素
* 元素个数未知，也无法使用随机访问的方式

## 方法一：保存每个节点地址

### 1.遍历链表

访问并保存所有链表中节点的地址，并保存在向量中。

### 2.随机访问向量

使用随机访问的方法访问倒数第n个元素的前驱，并修改其指向的方向。

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        vector<ListNode*> ptnd;
        //遍历链表，保存指针
        for(ListNode* i = head; i != NULL;i = i->next){
            ptnd.push_back(i);
        }
        //计算要修改next指针的位置
        int num = ptnd.size() - n - 1;
        //当n = -1时，需要将第一个元素去掉，要特殊处理
        if(num == -1) return head -> next;
        ptnd[num] -> next = ptnd[num] -> next ->next;
        return head;
    }
};
```

### 4.结果

**Runtime:**

4 ms, faster than 100.00% of C++ online submissions for Remove Nth Node From End of List.

**Memory Usage:**

8.7 MB, less than 96.3% of C++ online submissions for Remove Nth Node From End of List.

## 方法二：等尺度法

### 1.建立两个指针

用于存放前后两个的节点地址。

### 2.两指针间隔为n

将其中一个指针移至第n个元素，这样，两个指针的间隔为n。

### 3.同时后移两个指针

将两个指针同时后移，当第一个指针指向链表的尾部时，第二个指针指向的是链表的倒数第n个元素。

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode** t1 = &head;ListNode* t2 = head;
        //第一次移动至第n个位置处
        for(int i = 1; i< n; i++){
            t2 = t2 -> next;
        }
        //第二次，将两个指针一起移动
        for(;t2 -> next != NULL;t2 = t2->next){
            t1 = &((*t1) -> next);
        }
        //修改当前指针的指向
        *t1 = (*t1) -> next;
        return head;
    }
};
```

### 5.结果

**Runtime:**

4 ms, faster than 100.00% of C++ online submissions for Remove Nth Node From End of List.

**Memory Usage:**

 8.4 MB, less than 100.00% of C++ online submissions for Remove Nth Node From End of List.
