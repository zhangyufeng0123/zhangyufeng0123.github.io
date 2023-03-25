---
title: LeetCode 927. 三等分
categories:
  - LeetCode
tags:
  - 每日一题
abbrlink: 88c35860
date: 2023-03-20 12:13:13
---
# LeetCode 927. 三等分

# ****[927. 三等分](https://leetcode.cn/problems/three-equal-parts/) (20221006每日一题)**

## 题意

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/927.png)

给定一个由 `0`和 `1`组成的数组 `arr` ，将数组分成  **3 个非空的部分**，使得所有这些部分表示相同的二进制值。

如果可以做到，请返回**任何**`[i, j]`，其中 `i+1 < j`，这样一来：

- `arr[0], arr[1], ..., arr[i]` 为第一部分；
- `arr[i + 1], arr[i + 2], ..., arr[j - 1]` 为第二部分；
- `arr[j], arr[j + 1], ..., arr[arr.length - 1]` 为第三部分。
- 这三个部分所表示的二进制值相等。

如果无法做到，就返回 `[-1, -1]`

注意，在考虑每个部分所表示的二进制时，应当将其看作一个整体。例如，[1,1,0] 表示十进制的 6，而不会是 3。此外，前导零也是被允许的，所以 [0,1,1] 和 [1,1] 表示相同的值。

这道题我将数组直接看作一个字符串，然后题目要求我们将这个字符串分成三份，这三份所形成的二进制的值要想等。

## 思路

二进制的值的大小取决于该二进制中有效1的多少以及有效位数，如果有效位数越大，那么值也越大。

首先，我们得先确定这三部分的字符串是否可能会相等，最简单的方法就是判断1的个数，如果1的个数不是3的倍数，那么这个数组再怎么分割也不可能形成三个相等的二进制。因此我们可以先从1的个数来着手判断是否能分成三等份。

接下来我们将字符串分成三等份，每份中的1的个数相同，然后我们将相邻之间的0都放到前一份作为后导0，为什么这样放呢？是因为如果放到前导0毫无作用，前导0在值上面不起作用，放后导0如果值超过了，可以将后导0放到后一部分作为前导0降低二进制的值。

这样分完之后，我们统计三部份每份的后导0，因为最后一部分的0不能分给其他作为前导0，因此如果前两部分的后导0的个数只要有一个小于最后一份的后导0个数，那么这三等份就不可能是相等的，如果前两部分的后导0个数大于最后一份的，那么我们就将多余的0往后移作为前导0。

这样处理完之后，我们将每个二进制字符串的前导0删掉，这样剩下来的就是二进制的有效数，我们对比三个字符串，如果都相等，那么这个二进制数组可以三等分相等的二进制。

## 代码

```cpp
class Solution {
public:
    vector<int> threeEqualParts(vector<int>& arr) {
        vector<int> res(2, -1);
        int sum = 0;
        for(auto & a : arr) sum += a;
        if(sum % 3) return res;
        if(sum == 0){
            res[0] = 0, res[1] = 2;
            return res;
        }
        int zero[3] = {0}, tmp = 0, len[3];
        int idx = 0;
        string str[3];
        //先把二进制根据1分配，后面的分配后导0
        for(int i = 0; i < arr.size(); i++){
            if(arr[i]){
                tmp++;
                if(tmp == sum / 3 + 1) {
                    idx++;
                    tmp = 1;
                }
            }
            str[idx] += char('0' + arr[i]);
        }
        for(int i = 0; i < 3; i++){
            len[i] = str[i].size();
        }
        for(int i = 0; i < 3; i++){
            for(int j = str[i].size() - 1; j >= 0; j--){
                if(str[i][j] == '0') zero[i]++;
                else break;
            }
        }
        if(zero[0] < zero[2] || zero[1] < zero[2]) return res;
        for(int i = 0; i < 3; i++){
            while(str[i][0] == '0') str[i].erase(str[i].begin() + 0);
        }
        for(int i = 0; i < 2; i++){
            for(int j = 0; j < zero[i] - zero[2]; j++) str[i].pop_back();
        }
        if(str[0] != str[1] || str[0] != str[2]) return res;
        res[0] = len[0] - (zero[0] - zero[2]) - 1;
        res[1] = len[1] - (zero[1] - zero[2]) + len[0];
        return res;
    }
};
```

## 结果

![Result](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/927-1.png)