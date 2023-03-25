---
title: "LeetCode 895.\_最大频率栈"
categories:
  - LeetCode
tags:
  - 每日一题
  - 模拟
abbrlink: b7832ff7
date: 2023-03-20 14:58:34
---
# **[895. 最大频率栈](https://leetcode.cn/problems/maximum-frequency-stack/description/)(20221130每日一题)**

## 题面

设计一个类似堆栈的数据结构，将元素推入堆栈，并从堆栈中弹出**出现频率**最高的元素。

实现 `FreqStack` 类:

- `FreqStack()` 构造一个空的堆栈。
- `void push(int val)` 将一个整数 `val` 压入栈顶。
- `int pop()` 删除并返回堆栈中出现频率最高的元素。
    - 如果出现频率最高的元素不只一个，则移除并返回最接近栈顶的元素。

## Sample

### Input：

```cpp
["FreqStack","push","push","push","push","push","push","pop","pop","pop","pop"],
[[],[5],[7],[5],[7],[4],[5],[],[],[],[]]
```

### Output:

```cpp
[null,null,null,null,null,null,null,5,7,5,4]
```

- `0 <= val <= 1e9`
- `push` 和 `pop` 的操作数不大于 `2 * 1e4`。
- 输入保证在调用 `pop` 之前堆栈中至少有一个元素。

## 思路

题目中涉及到两个要点，一个是栈，一个是频率，即出现次数。题目要求我们实时弹出频率最高的那个元素，如果频率相同的元素，那就弹出先压入的元素。

那么我们怎么来确定每个元素的频率呢？考虑到元素范围是[0,1e9],那么如果使用数组来维护每个元素的频率，那就有可能会爆内存。后面显示push和pop的操作数不大于2e4，那么就代表元素种类最多是2e4，我们可以用哈希表来存。每push进一个元素的时候，我们就对其对应的出现次数进行+1操作，每pop一次时，我们对其对应的出现次数进行-1操作。这样我们就完成了对频率的维护。

剩下的问题是如何弹出频率最高的元素。我们目前已经完成了对push进来的数和pop出去的数的频率维护，我们要优先弹出频率最高的数。那么我们可以用数组来存储栈，数组的索引值代表着出现的频率，那么每当进行pop操作时，我们就优先弹出数组末尾的stack，因为数组末尾stack中存储的数都是频率最高的。

## 代码

```cpp
class FreqStack {
private:
    vector<stack<int>> vst;
    unordered_map<int, int> hash;

public:
    FreqStack() {
        vst.clear();
    }
    
    void push(int val) {
        if(hash[val] == vst.size()){
            vst.push_back({});
        }
        vst[hash[val]].push(val);
        hash[val]++;
    }
    
    int pop() {
        while(vst.back().empty()){
            vst.pop_back();
        }
        int e = vst.back().top();
        vst.back().pop();
        hash[e]--;
        return e;
    }
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(val);
 * int param_2 = obj->pop();
 */
```