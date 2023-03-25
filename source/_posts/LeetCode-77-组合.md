---
title: LeetCode 77. 组合
categories:
  - LeetCode
tags:
  - 排列组合
  - Dfs
abbrlink: d1540be5
date: 2023-03-20 12:09:38
---
# LeetCode 77. 组合

# ****[77. 组合](https://leetcode.cn/problems/combinations/)****

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/77.png)

## 题意

这道题的意思就是一道排列组合的问题，即求 $C^k_n$这种数学问题，但是我们不是求值，而是求排列组合中每一种的组合。这样的题显然是一种枚举问题，遇到枚举问题 $一般$是用dfs和bfs的方法。在这道题中，我用了一个剪枝策略来加速寻找每一种组合。

如果剩下的数字个数加上当前数组大小小于要求值k，则返回

## 代码

```cpp
class Solution {
    vector<vector<int>> res;
public:
    void dfs(vector<int> arr, int index, int n, int sum, int k){
        if(sum == k){
            this->res.emplace_back(arr);
            return;
        }
        if(index > n) return;
        if(sum + n - index + 1 < k) return;
        dfs(arr, index + 1, n, sum, k);
        arr.emplace_back(index);
        dfs(arr, index + 1, n, sum + 1, k);
    }

    vector<vector<int>> combine(int n, int k) {
        vector<int> arr;
        dfs(arr, 1, n, 0, k);
        return this->res;
    }
};
```