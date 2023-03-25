---
title: "LeetCode 2469.\_温度转换"
categories:
  - LeetCode
tags:
  - 每日一题
abbrlink: e80ecef2
date: 2023-03-21 13:28:10
---

## **[2469. 温度转换](https://leetcode.cn/problems/convert-the-temperature/description/)**

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230321132248.png)

## Sample

### Input

```cpp
celsius = 36.50
```

### Output

```cpp
[309.65000,97.70000]
```

### Note

• `0 <= celsius <= 1000`

## 做法

单纯的输入输出

## 代码

```cpp
class Solution {
public:
    vector<double> convertTemperature(double celsius) {
        vector<double> res(2, 0);
        res[0] = celsius + 273.15;
        res[1] = celsius * 1.80 + 32.00;
        return res;
    }
};
```