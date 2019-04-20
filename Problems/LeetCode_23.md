# [LeetCode] 23. Merge k Sorted Lists

## 题目描述

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example:**

```C++
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

## 要点

* 链表合并

## 方法一：两链表合并

最初方法是想维护一个指向k个头结点的的数组，然后每次取出最小值，并将相应队列的下一个元素添加进去。但是发现使用这种方法在每次选出最小值的时候也会进行大量的比较操作，故放弃，采用本方法。

### 1.建立合并两个链表的函数

在题目[21. Merge Two Sorted Lists](https://github.com/liyupeng341/LeetCode/blob/master/Problems/LeetCode_21.md)中已经建立过。

### 2.合并链表

将第2、3...n个链表以每次一个的方式与第一个链表合并。

*这里有人用分治法进行合并，先两两和并，再进行下一轮合并，但是两个方式合并的次数是相同的。*

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
    //题目21中合并两个链表的子程序
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *res = new ListNode(INT_MIN);
        ListNode *temp = res;
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
        temp -> next = l1?l1:l2;
        return res -> next;
    }
    //本题的函数
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        //特殊情况的处理
        if(lists.size() < 1){ return {};}
        ListNode temp(INT_MIN);
        //建立头结点
        temp . next = lists[0];
        ListNode *res = &temp;
        //进行n-1次合并
        int i = 1;
        while(i < lists.size()){
            temp . next = mergeTwoLists(temp . next,lists[i]);
            i++;
        }
        return res -> next;
    }
};
```

### 4.结果

**Runtime:**

 168 ms, faster than 27.33% of C++ online submissions for Merge k Sorted Lists.

**Memory Usage:**

 11.1 MB, less than 99.14% of C++ online submissions for Merge k Sorted Lists.

## 方法二：两链表合并

本方法和方法一只有实现形式不同，同时本方法使用了**两两合并**的方法。也学习到了使用**递归方式**进行两链表合并的方法。本方法来自LeetCode讨论区的[这里](https://leetcode.com/problems/merge-k-sorted-lists/discuss/10531/Sharing-my-straightforward-C%2B%2B-solution-without-data-structure-other-than-vector)。

## 1.代码

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
    //合并k链表方法
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        //特殊情况处理
        if(lists.empty()){
            return nullptr;
        }
        //合并最前面的两个链表，并保存在最后
        while(lists.size() > 1){
            lists.push_back(mergeTwoLists(lists[0], lists[1]));
            lists.erase(lists.begin());
            lists.erase(lists.begin());
        }
        return lists.front();
    }
    //递归方法合并两个链表
    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
        if(l1 == nullptr){
            return l2;
        }
        if(l2 == nullptr){
            return l1;
        }
        if(l1->val <= l2->val){
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }
        else{
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```

### 2.结果

**Runtime:**

 244 ms, faster than 22.03% of C++ online submissions for Merge k Sorted Lists.

**Memory Usage:**

 11.3 MB, less than 98.90% of C++ online submissions for Merge k Sorted Lists.

## 方法三：优先队列

优先队列是一种抽象数据结构，用于获得最小值。

本方法来自LeetCode讨论区的[这里](https://leetcode.com/problems/merge-k-sorted-lists/discuss/10527/Difference-between-Priority-Queue-and-Heap-and-C%2B%2B-implementation)。

### 1.建立优先队列

建立一个有k个元素的优先队列，用于获取当前k个元素的最小值。

### 2.维护优先队列

每次获取该队列最小值，并将其移出队列，然后新添加一个元素。

### 3.终止条件

当优先队列中没有元素时，任务完成，结束程序。

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
    //优先队列比较环节
    struct comp{
        bool operator()(ListNode* l1, ListNode* l2){
            return l1 -> val >l2 ->val;
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        //建立队列
        priority_queue<ListNode*,vector<ListNode*>,comp> q;
        for(int i = 0; i < lists.size(); i++){
            //将空指针去除
            if(lists[i])q.push(lists[i]);
        }
        //处理特殊情况
        if(q.empty()) return NULL;
        //建立头结点res与尾部节点temp
        ListNode res(INT_MIN);
        ListNode* temp = &res;
        temp -> next = q.top();
        temp = temp -> next;
        q.pop();
        if(temp -> next)q.push(temp -> next);
        //维护堆的过程
        while(!q.empty()){
            temp -> next = q.top();
            temp = temp -> next;
            q.pop();
            //只将有元素的指针移入堆中
            if(temp -> next)q.push(temp -> next);
        }
        return res.next;
    }
};
```

### 5.结果

**Runtime:**

 28 ms, faster than 92.85% of C++ online submissions for Merge k Sorted Lists.

**Memory Usage:**

 11 MB, less than 99.63% of C++ online submissions for Merge k Sorted Lists.

## 方法四：最小堆方法

最小堆方法原理与优先队列相同。最小堆是优先队列的一种实现的数据结构。本方法来自LeetCode讨论区的[这里](https://leetcode.com/problems/merge-k-sorted-lists/discuss/10527/Difference-between-Priority-Queue-and-Heap-and-C%2B%2B-implementation)。本方法只是将优先队列替换为最小堆，修改了访问方式和组建方法，其余部分均未改变。

### 1.代码

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
    //最小堆比较环节
    static bool comp(ListNode* l1, ListNode* l2){
            return l1 -> val >l2 ->val;
        }
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        //建立向量
        vector<ListNode*> q;
        for(int i = 0; i < lists.size(); i++){
            //将空指针去除
            if(lists[i])q.push_back(lists[i]);
        }
        //建立最小堆
        make_heap(q.begin(),q.end(),comp);
        //处理特殊情况
        if(q.empty()) return NULL;
        //建立头结点res与尾部节点temp
        ListNode res(INT_MIN);
        ListNode* temp = &res;
        temp -> next = q.front();
        temp = temp -> next;
        //最小堆的访问方式与优先队列略微不同
        pop_heap(q.begin(),q.end(),comp);
        q.pop_back();
        if(temp -> next){
            q.push_back(temp -> next);
            push_heap(q.begin(),q.end(),comp);
            }
        //维护最小堆的过程
        while(!q.empty()){
            temp -> next = q.front();
            temp = temp -> next;
            pop_heap(q.begin(),q.end(),comp);
            q.pop_back(); 
            //只将有元素的指针移入堆中
            if(temp -> next){
                q.push_back(temp -> next);
                push_heap(q.begin(),q.end(),comp);
            }
        }
        return res.next;
    }
};
```

### 2.结果

**Runtime:**

 44 ms, faster than 39.36% of C++ online submissions for Merge k Sorted Lists.

**Memory Usage:**

 10.8 MB, less than 99.75% of C++ online submissions for Merge k Sorted Lists.
