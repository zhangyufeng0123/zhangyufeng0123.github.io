---
title: LeetCode 1637.两点之间不包含任何点的最宽垂直区域
categories:
  - LeetCode
tags:
  - 排序
abbrlink: 8e39a6b9
date: 2023-03-30 18:33:44
---

## **[1637. 两点之间不包含任何点的最宽垂直区域](https://leetcode.cn/problems/widest-vertical-area-between-two-points-containing-no-points/description/)**

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230330182639.png)

## Sample

## Input

```cpp
[[8,7],[9,9],[7,4],[9,7]]
```

### Output

```cpp
1
```

### Example

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230330182733.png)

### 范围

- `n == points.length`
- `2 <= n <= 1e5`
- `points[i].length == 2`
- `0 <= xi, yi <= 1e9`

## 思路

这道题乍看是一道中等题，但其实是一道简单题。

**最宽垂直区域**用另一种说法就是两个点的横坐标的差。题意这样一转换，难度就降低了。

## 做法

将点根据横坐标进行从大到小进行排序，然后遍历一边即可。

## 代码

```cpp
class Solution {
public:
    int maxWidthOfVerticalArea(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), [&](vector<int> &A, vector<int> &B){
            return A[0] < B[0];
        });
        int res = 0;
        for(int i = 1; i < points.size(); i++){
            res = max(res, points[i][0] - points[i - 1][0]);
        }
        return res;
    }
};
```