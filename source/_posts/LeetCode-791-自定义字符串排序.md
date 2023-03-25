---
title: "LeetCode 791.\_自定义字符串排序"
categories:
  - LeetCode
tags:
  - 每日一题
abbrlink: 375eda24
date: 2023-03-20 14:52:47
---
# **[791. 自定义字符串排序](https://leetcode.cn/problems/custom-sort-string/)(20221113 每日一题)**

## 题面

给定两个字符串 `order` 和 `s` 。`order` 的所有单词都是 **唯一** 的，并且以前按照一些自定义的顺序排序。对 `s` 的字符进行置换，使其与排序的 `order` 相匹配。更具体地说，如果在 `order` 中的字符 `x` 出现字符 `y` 之前，那么在排列后的字符串中， `x` 也应该出现在 `y` 之前。返回 *满足这个性质的 `s` 的任意排列* 。

## Sample

### Input

```cpp
order = "cba", s = "abcd"
```

### Output

```cpp
"cbad"
```

## 思路

题目中给了我们两个字符串，一个优先级字符串，一个需要处理的字符串。从题目我们可以得出，我们需要处理的是有优先级定义的字符，对于其余字符我们可以不做处理，可以放在字符串最前面、可以放在字符串最后面、可以穿插在字符串之间。那么我们怎么处理这个有优先级的字符呢？

### 1

第一种想法就是我们先对$order$字符串中的字符定义优先级，其余字符的优先级都定义为最高优先级，然后对$s$字符串中的字符根据我们刚刚定义的优先级进行排序。但是这样有一个问题，这个想法涉及到排序，那么复杂度是$O(nlogn)$级别的，如果字符串长度太长会有一定的影响（但是题目中已经明确给出了字符串长度小于等于200，那这个思路也是可行的）。

### 2（优化）

第二种想法就是一种$O(n)$的思路，我们没有必要使用排序来获得最终字符串，我们首先统计字符串$s$中每个字符有多少个，然后遍历$order$字符串，遍历到一个字符我们就将$s$中所有该字符加入目标字符串中，最后将没有出现在$order$字符串中但是出现在$s$字符串中的字符加入目标字符串即可。这个想法我们只要遍历两遍字符串$s$。

## 代码

```cpp
class Solution {
public:
    string customSortString(string order, string s) {
        int O[26] = {0};
        int letters[26] = {0};
        string res;
        for(int i = 0; i < s.size(); i++) letters[s[i] - 'a']++;
        for(int i = 0; i < order.size(); i++){
            O[order[i] - 'a'] = 1;
            while(letters[order[i] - 'a']){
                res += order[i];
                letters[order[i] - 'a']--;
            }
        }
        for(int i = 0; i < 26; i++){
            if(!O[i]){
                while(letters[i]) {
                    res += char('a' + i);
                    letters[i]--;
                }
            }
        }
        return res;
    }
};
```