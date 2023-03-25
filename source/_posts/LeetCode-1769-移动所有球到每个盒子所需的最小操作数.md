---
title: LeetCode 1769.移动所有球到每个盒子所需的最小操作数
categories:
  - LeetCode
tags:
  - 每日一题
  - 前缀和
abbrlink: 53590df
date: 2023-03-20 14:59:53
---

# **[1769. 移动所有球到每个盒子所需的最小操作数](https://leetcode.cn/problems/minimum-number-of-operations-to-move-all-balls-to-each-box/description/)(20221202每日一题)**

# 题面

有 `n` 个盒子。给你一个长度为 `n` 的二进制字符串 `boxes` ，其中 `boxes[i]` 的值为 `'0'` 表示第 `i` 个盒子是 **空** 的，而 `boxes[i]` 的值为 `'1'` 表示盒子里有 **一个** 小球。

在一步操作中，你可以将 **一个** 小球从某个盒子移动到一个与之相邻的盒子中。第 `i` 个盒子和第 `j` 个盒子相邻需满足 `abs(i - j) == 1` 。注意，操作执行后，某些盒子中可能会存在不止一个小球。

返回一个长度为 `n` 的数组 `answer` ，其中 `answer[i]` 是将所有小球移动到第 `i` 个盒子所需的 **最小** 操作数。

每个 `answer[i]` 都需要根据盒子的 **初始状态** 进行计算。

# Sample

## Input：

```cpp
boxes = "110"
```

## Output:

```cpp
[1,1,3]
```

- `n == boxes.length`
- `1 <= n <= 2000`
- `boxes[i]` 为 `'0'` 或 `'1'`

# 思路

根据题目的意思，这是让我们求把所有的球移到一个位置所需要的最少次数，一次只能把一个球移到邻居。知道这个步骤之后，我们只要知道每个位置的左边的球和右边的球分别移到这个位置所需要多少次即可。那么我们怎么计算呢？如果每计算一个索引值时都遍历一次数组，那需要$O(n^2)$的时间复杂度，虽然本题中n的范围是`[1,2000]`，时间是足够的，但是这里有另一个更低的时间复杂度的方法。我们需要维护两个数组来维护数据，一个数组来维护这个位置左边所有的球到这个位置需要的次数和右边所有的球到这个位置需要的次数，怎么维护呢？这里就涉及到前缀和的思想。

# 代码

```cpp
class Solution {
public:
    vector<int> minOperations(string boxes) {
        int len = boxes.size();
        vector<int> l(len, 0), r(len, 0);
        int balls = 0;
        if(boxes.front() == '1') balls++;
        for(int i = 1; i < len; i++){
            l[i] = l[i - 1] + balls;
            if(boxes[i] == '1') balls++;
        }
        balls = 0;
        if(boxes.back() == '1') balls++;
        for(int i = len - 2; i >= 0; i--){
            r[i] = r[i + 1] + balls;
            if(boxes[i] == '1') balls++;
        }
        for(int i = 0; i < len; i++){
            l[i] += r[i];
        }
        return l;
    }
};
```