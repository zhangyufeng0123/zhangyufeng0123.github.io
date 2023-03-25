---
title: LeetCode 788.旋转数字
categories:
  - LeetCode
tags:
  - LeetCode每日一题
abbrlink: 28252a46
date: 2023-03-17 19:35:32
---
# LeetCode 788.旋转数字(Mid)

# 788 旋转数字（20220925每日一题）

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/1.png)

## 题意

题目中已经给出了大致意思，要想数字每个位上的数都旋转后还能成为数字，那么这个数字必须包含且只能包含数字0，1，2，5，6，8，9。那么为什么前十个数中1，2，8，10不包含在旋转数字中呢？因为这4个数字旋转之后还是1，2，8，10，数值不变，这违反了旋转数字的定义。从这我们可以看出，要想数字旋转之后数值要改变，不能只包含1，2，8，0，还必须至少包含2，5，6，9中的一个，那么我们只要判断从1到n中的数字中有几个数是由上述8个数字组成，并且至少包含一个2，5，6，9.

```cpp
class Solution {
public:
    int rotatedDigits(int n) {
        int res = 0;
        vector<int> f(10, 0);
        f[2] = 1;
        f[5] = 1;
        f[6] = 1;
        f[9] = 1;

        f[1] = 2;
        f[0] = 2;
        f[8] = 2;
        for(int i = 1; i <= n; i++){
            int t = i;
            int a = 0, b = 0;
            while(t){
                if(f[t % 10] == 1) a = 1;
                else if(f[t % 10] == 2) b = 1;
                else break;
                t /= 10;
            }
            if(!t && a) res++;
        }
        return res;
    }
};
```