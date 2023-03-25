---
title: LeetCode 904. 水果成篮
categories:
  - LeetCode
tags:
  - 双指针
  - 每日一题
abbrlink: f4d40e11
date: 2023-03-20 14:23:33
---
# [904. 水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)(20221017 每日一题)

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/904-1.png)
## 题意

题目给我们一个数组，让我们找到最长的子串，使得这个子串中所包含的数字种类最多只有两个。

## 思路

解决这种题，简单的就是利用双指针，一个在前，一个在后，在前的指针负责往队列里加数字，如果加完数字之后发现队列里的数字种类大于两种，那么我们就用后指针来弹出队列的数字，直到队列的数字种类小于等于2。

## Code

```cpp
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        int first = 0, last = 1;
        unordered_map<int, int> hash;
        hash[fruits[0]]++;
        int sum = 1, res = 0;
        while(last < fruits.size()){
            hash[fruits[last]]++;
            if(hash[fruits[last]] == 1){
                res = max(res, last - first);
                sum++;
                while(sum > 2){
                    hash[fruits[first]]--;
                    if(hash[fruits[first]] == 0){
                        sum--;
                    }
                    first++;
                }
            }
            last++;
        }
        res = max(res, last - first);
        return res;
    }
};
```

## 结果

![Result](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/904-2.png)