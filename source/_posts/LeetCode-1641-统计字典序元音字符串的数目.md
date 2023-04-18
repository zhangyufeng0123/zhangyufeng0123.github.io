---
title: LeetCode 1641.统计字典序元音字符串的数目
categories:
  - LeetCode
tags:
  - 每日一题
  - 数学推导
abbrlink: 9207185a
date: 2023-03-29 15:24:08
---

## [1641.**统计字典序元音字符串的数目**](https://leetcode.cn/problems/count-sorted-vowel-strings/description/)

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230329150717.png)

## Sample

### Input

```cpp
n = 2
```

### Output

```cpp
15
```

### Explain

```cpp
仅由元音组成的 15 个字典序字符串为
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"]
注意，"ea" 不是符合题意的字符串，因为 'e' 在字母表中的位置比 'a' 靠后
```

### 数据范围

• `1 <= n <= 50`

## 思路

看这道题，第一眼是一道中等题，不太好暴力，再看数据范围，好像又可以暴力，毕竟范围是`[1,50]`。暴力的做法就是用`dfs`搜索所有可能，但事后面自己列了一下数组，发现有一种更简单的方法

首先，如果`m=1`，那么数组就是`[1,1,1,1,1]`，那如果`m=2`的时候，字母`’a’`后面能跟的就是`m-1`数组中的所有组合，因为题目前要求是相同或者小，因此`’a’`字母后面能跟`m-1`数组的所有组合，那么`’e’`字母的后面就只能跟`n-1`数组中的除了`’a’`开头的其他组合。以此类推，我们不断更新`m`直到`m=n`，这个时候的数组就是以每一种元音字母作为开头它的种数。

## 代码

```cpp
class Solution {
public:
    int countVowelStrings(int n) {
        vector<int> res(5, 1);
        for(int i = 0; i < n - 1; i++){
            for(int j = 0; j < 4; j++){
                res[j] = accumulate(res.begin() + j, res.end(), 0);
            }
        }
        return accumulate(res.begin(), res.end(), 0);
    }
};
```