---
title: 环形链表II
categories:
  - 灵茶山艾府学习笔记
tags:
  - 链表
  - 快慢指针
abbrlink: 7ad2c734
date: 2023-03-26 15:33:52
---
## **[876.链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/description/)**

这道题就是一道典型的快慢指针的问题，让我们找一串链表的中间位置。

暴力的做法是先扫一遍，看看这个链表的长度，然后根据长度找到目标值。

有没有更巧妙的做法呢？有，就是快慢指针，因为我们要求链表的中间节点，那么我们设置两个指针，一个指针一次走两格，另一个指针一次走一格。这样我们就能保证走一格的指针始终是走两格的指针的中间，当走得快的指针走到终点的时候返回走得慢的指针即可。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* p = head;
        while(p->next != NULL && p->next->next != NULL){
            head = head->next;
            p = p->next->next;
        }
        if(p->next == NULL) return head;
        else return head->next;
    }
};
```

## **[141.环形链表](https://leetcode.cn/problems/linked-list-cycle/description/)**

证明链表是否有环，我们只要根据链表的next指针一直往下走就行，如果遇到空指针，那么说明没有环，但是我们什么时候推出寻找next呢？万一有环不是得一直寻找下去吗？所以这个方法不行。

我们可以用快慢指针来解决这个问题。

快指针还是一次走两格，满指针一次走一格。

用递归法证明，快慢指针一定会相遇：

1. 快指针与慢指针之间差一步。此时继续往后走，满指针前进一步，快指针前进两步，两者相遇。
2. 快指针与满指针之间差两步。此时继续往后走，慢指针前进一步，快指针前进两步，两者之间相差一步，转化为第一种情况。
3. 快指针与慢指针之间差N步。此时继续往后走，慢指针前进一步，快指针前进两步，两者之间相差（N+1-2）即N-2步。重复这个过程，直到快指针和慢指针相遇。
- 推导：慢指针进入环后，快指针最多多绕一个圈
    
    快指针F先进环，慢指针S后进
    
    假设慢指针进环那一刻快指针差m步能追上（0≤m≤Length环），根据上边的结论，两个指针走m次就会相遇了
    
    因为m<Length环，所以快指针在慢指针进环那一刻最多比慢指针多绕一个圈。
    

```cpp
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
        if(head == NULL) return false;
        ListNode* p = head;
        while(p->next != NULL && p->next->next != NULL){
            p = p->next->next;
            head = head->next;
            if(p == head) return true;
        }
        return false;
    }
};
```

## **[142.环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/description/)**

这道题有一个证明：

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326144715.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326144715.png)

其中a是head到入口的距离，b是入口到快慢指针相遇的距离，c是相遇点到入口的长度。

这样我们就能知道3个关系式

1. 环长:b+c
2. 慢指针移动距离:a+b
3. 快指针移动距离:a+b+k(b+c)

因为快指针移动速度是慢指针移动速度的两倍

所以: a+b+k(b+c) = 2(a+b)

⇒a+b+b+c+(k-1)(b+c)=a+a+b+b

⇒a=(k-1)(b+c)-c

⇒a=(k-1)*Length环+c

因此当快慢指针相遇后，一个一次只移动一格的指针到达入口时，慢指针也刚好从相遇点到入口。这时两个指针就相遇了。

```cpp
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
    ListNode *detectCycle(ListNode *head) {
        if(head == NULL) return head;
        ListNode *p = head, *ans = head;
        while(head->next != NULL && head->next->next != NULL){
            head = head->next->next;
            p = p->next;
            if(head == p) break;
        }
        if(head->next == NULL || head->next->next == NULL) return NULL;
        while(ans != p){
            ans = ans->next;
            p = p->next;
        }
        return ans;
    }
};
```

## **[143.重排链表](https://leetcode.cn/problems/reorder-list/description/)**

看到题目的要求，让我们将前面一半的链表以及后面一般的链表倒置进行合并。

这样我们就需要用到上一篇用到的反转链表的方法，之后再进行合并。

合并的具体步骤如下：

1. 两个初始链表，head1、head2是两个链表的头指针，s记录合并链表的当前位置

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326152438.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326152438.png)

1. 第一个链表的头指针向后移动一位

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326152510.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326152510.png)

1. 我们更改当前节点的next指针，将next指针指向另外一条链表的头指针

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326152738.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326152738.png)

1. 移动s指针，将s移到s的next位置，再将head2指针往后移一个

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326153033.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326153033.png)

1. 继续移动s指针，再回到第一步

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326153129.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230326153129.png)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* revserseList(ListNode* head){
        ListNode *t = NULL, *p = head;
        while(head){
            head = head->next;
            p->next = t;
            t = p;
            p = head;
        }
        return t;
    }

    void reorderList(ListNode* head) {
        if(head->next == NULL) return;
        ListNode* f = head, *s = head, *t = head;
        while(f->next != NULL && f->next->next != NULL){
            f = f->next->next;
            t = s;
            s = s->next;
        }
        ListNode *list1;
        if(f->next == NULL){
            t->next = NULL;
            list1 = revserseList(s);
        }else{
            t = s;
            s = s->next;
            t->next = NULL;
            list1 = revserseList(s);
        }
        
        f = head, s = head, t = head;
        ListNode *ans = head;
        while(head){
            s = head;
            head = head->next;
            s->next = list1;
            s = s->next;
            list1 = list1->next;
            s->next = head;
        }
        if(list1){
            s->next = list1;
        }
        head = ans;
    }
};
```