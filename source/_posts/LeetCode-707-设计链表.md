---
title: LeetCode 707.设计链表
categories:
  - LeetCode
tags:
  - 链表
  - LeetCode每日一题
abbrlink: 9ad52be0
date: 2023-03-17 15:46:54
---
# LeetCode 707.设计链表(mid)

# 707 设计链表（20220923每日一题）

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/Untitled.png)

从题目中我们可以看到本题有以下几个功能：

1. 获取链表中位列index的值
2. 在链表的头部添加节点
3. 在链表的尾部添加节点
4. 在链表中的指定索引地添加节点
5. 删除指定索引的节点

题目让我们不能使用内置的LinkedList库，那么我们可以用数组和链表来维护。由于该题有频繁的增加节点和删除节点，那么用数组来维护需要耗费大量的时间来维护index，因此我们选择链表来维护本题的“链表”。

## 链表的构造

由于是一个链表，那么节点中所需要的元素至少有两个，一个值val，和一个指向下一个节点的*next，最好就是添加一个构造函数，这样方便快速生成新节点

```cpp
struct List{
        int val;
        List *next;
        List(int val) : val(val), next(NULL){}
    };
```

## 链表的初始化

当我们创建一个链表的时候，我们需要对其进行初始化，即构造函数，我们要对其成员进行一系列的初始化，在本题中我们需要初始化的元素只有结构体List，以及一个链表的长度，这个长度方便我们之后判断是否可以删除某个节点或者是否可以将节点加入指定的index

```cpp
MyLinkedList() {
        this->root = new List(-1);
        this->len = 0;
    }
```

我们创建的链表是自带头节点的链表，这类链表方便进行头插法（但是这题我的思路还是尾插法）

## 获取值

提供一个索引值index，返回索引所在的节点的val值，首先我们要先判断这个索引值index是否有效，如果无效则直接返回-1。

```cpp
int get(int index) {
        if(index >= this->len || index < 0) return -1;
        List *head = this->root->next;
        while(index--){
            head = head->next;
        }

        return head->val;
    }
```

## 在链表头部添加节点

在添加节点的三个功能中，添加头部节点是最方便的，因为相比于其他两个，头部添加节点不需要找位置，可以直接添加。

```cpp
void addAtHead(int val) {
        List *node = new List(val);
        node->next = this->root->next;
        this->root->next = node;
        this->len++;
    }
```

## 在链表尾部添加节点

添加尾部节点，如果我们用一个尾部标记tail来维护尾部节点，那么这个我们也能够快速添加，但是我没有用tail来维护，那么我们就要遍历到最后链表的最后一位以便于添加节点。

```cpp
void addAtTail(int val) {
        List *head = this->root;
        while(head->next != NULL) head = head->next;
        List *node = new List(val);
        head->next = node;
        this->len++;
    }
```

## 在给定索引值前添加节点

这部分内容相比于前两个来说，只能够通过遍历来获取要添加的位置。

题目中对给定索引值添加节点指定了要求，首先，如果这个索引值大于链表的长度，那么我们将取消这次添加节点。如果这个索引值小于0，那么我们就将这个节点添加在头部，其余的我们需要遍历来获取节点添加的位置。

```cpp
void addAtIndex(int index, int val) {
        if(index > this->len) return;
        List *node = new List(val);
        List *head = this->root;
        while(index--){
            head = head->next;
        }
        node->next = head->next;
        head->next = node;
        this->len++;
    }
```

## 删除指定索引值的节点

题目中对删除节点指定了要求，如果索引值index有效，那么我们可以顺利删除，否则，我们将因为没有对应的节点可以删除而退出这个删除函数

```cpp
void deleteAtIndex(int index) {
        if(index < 0 || index >= this->len) return;
        List *head = this->root;
        while(index--){
            head = head->next;
        }
        List *tmp = head->next;
        head->next = head->next->next;
        delete tmp;
        this->len--;
    }
```

## 完整的代码

```cpp
class MyLinkedList {
    struct List{
        int val;
        List *next;
        List(int val) : val(val), next(NULL){}
    };
    List *root;
    int len;
public:
    MyLinkedList() {
        this->root = new List(-1);
        this->len = 0;
    }
    
    int get(int index) {
        if(index >= this->len || index < 0) return -1;
        List *head = this->root->next;
        while(index--){
            head = head->next;
        }

        return head->val;
    }
    
    void addAtHead(int val) {
        List *node = new List(val);
        node->next = this->root->next;
        this->root->next = node;
        this->len++;
    }
    
    void addAtTail(int val) {
        List *head = this->root;
        while(head->next != NULL) head = head->next;
        List *node = new List(val);
        head->next = node;
        this->len++;
    }
    
    void addAtIndex(int index, int val) {
        if(index > this->len) return;
        List *node = new List(val);
        List *head = this->root;
        while(index--){
            head = head->next;
        }
        node->next = head->next;
        head->next = node;
        this->len++;
    }
    
    void deleteAtIndex(int index) {
        if(index < 0 || index >= this->len) return;
        List *head = this->root;
        while(index--){
            head = head->next;
        }
        List *tmp = head->next;
        head->next = head->next->next;
        delete tmp;
        this->len--;
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```