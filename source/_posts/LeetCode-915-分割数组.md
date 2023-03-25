---
title: LeetCode 915.分割数组
categories:
  - LeetCode
tags:
  - 每日一题
  - 前缀和
abbrlink: 2acaafb0
date: 2023-03-20 14:38:34
---
# LeetCode 915.分割数组（Medium）

# [915. 分割数组](https://leetcode.cn/problems/partition-array-into-disjoint-intervals/)（20221024每日一题）

## 题面

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/915-1.png)

### Sample

### Input

```cpp
nums = [5,0,3,8,6]
```

### Output

```cpp
3
```

## 思路

题目要求我们将一个数组分为左右两部分，并且左半边的最大值要小于等于右半边的最小值，那么我们需要用两个数组 $b, s$来维护这个最大值喝最小值， $b[i]$代表我维护的是在`[1,i`]之间的最大值，`s[i]`代表我维护的是在`[i,]`的最小值，之后我们只要遍历一次数组，当`b[i]>s[i + 1]`的时候我们就能找到我们想要找到的分界点。

## 代码

```cpp
class Solution {
public:
    int partitionDisjoint(vector<int>& nums) {
        int len = nums.size();
        vector<int> b(len, 0), s(len, 0);
        b[0] = nums[0];
        for(int i = 1; i < len; i++) b[i] = max(b[i - 1], nums[i]);
        s[len - 1] = nums[len - 1];
        for(int i = len - 2; i >= 0; i--) s[i] = min(s[i + 1], nums[i]);
        for(int i = 0; i < len - 1; i++){
            if(b[i] <= s[i + 1]) return i + 1;
        }
        return len;
    }
};
```

## 结果

![Result](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/915-2.png)