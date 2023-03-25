---
title: LeetCode 1326.灌溉花园的最少水龙头数目
categories:
  - LeetCode
tags:
  - 每日一题
abbrlink: 3f55d95f
date: 2023-03-20 15:01:45
---

# **[1326. 灌溉花园的最少水龙头数目](https://leetcode.cn/problems/minimum-number-of-taps-to-open-to-water-a-garden/description/)(20230221每日一题)**

## 题面

在 x 轴上有一个一维的花园。花园长度为 `n`，从点 `0` 开始，到点 `n` 结束。

花园里总共有 `n + 1` 个水龙头，分别位于 `[0, 1, ..., n]` 。

给你一个整数 `n` 和一个长度为 `n + 1` 的整数数组 `ranges` ，其中 `ranges[i]` （下标从 0 开始）表示：如果打开点 `i` 处的水龙头，可以灌溉的区域为 `[i -  ranges[i], i + ranges[i]]` 。

请你返回可以灌溉整个花园的 **最少水龙头数目** 。如果花园始终存在无法灌溉到的地方，请你返回 **-1** 。

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/1326.png)

## 输入

```cpp
n = 5, ranges = [3,4,1,1,0,0]
```

## 输出

```cpp
1
```

## 样例解释

`点 0 处的水龙头可以灌溉区间 [-3,3]
点 1 处的水龙头可以灌溉区间 [-3,5]
点 2 处的水龙头可以灌溉区间 [1,3]
点 3 处的水龙头可以灌溉区间 [2,4]
点 4 处的水龙头可以灌溉区间 [4,4]
点 5 处的水龙头可以灌溉区间 [5,5]
只需要打开点 1 处的水龙头即可灌溉整个花园 [0,5] 。`

## 思路

这道题看似是一道hard难度的题，真实难度感觉是一道medium难度的题。

题目的意思是用多个喷水口来覆盖整个花园，它的表达意思可能让我们难以下手，但是如果我们换一个说法，每一个喷水口不是`[i-range[i], i+range[i]]`，而是`[i, i+range[i]*2]`。这样子就能更好的理解了。

接下来的解题步骤就是我们先确定每个水龙头能灌溉的左右边界，然后确定每个左边界能够到达的最右边界。之后我们就从头开始遍历，先确定`0`位置能够到达的最右边界，然后我们定义`right`为当前最右边界，`right_last`为上次最右边界，然后每次从当前最右边界遍历到上次最右边界，如果在这个区间中能够到达的最右边界是小于`right`的值，那么说明水龙头不能灌溉所有的区间。否则就更新`right`和`right_last`这两个值，直到`right`值大于等于`n`。

## 代码

```cpp
class Solution {
public:
    int minTaps(int n, vector<int>& ranges) {
        vector<int> right_most(n+1, 0);
        for(int i = 0; i <= n; i++){
            int len = ranges[i];
            if(i >= len) right_most[i - len] = i + len;
            else right_most[0] = max(right_most[0], len + i);
        }

        int res = 0, right = 0, right_last = 0;
        while(right < n){
            int right_tmp = -1;
            for(int i = right; i >= right_last; i--){
                if(right_most[i] > right){
                    right_tmp = max(right_tmp, right_most[i]);
                }
            }
            if(right_tmp == -1) return -1;
            res++;
            right_last = right;
            right = right_tmp;
        }
        return res;
    }
};
```