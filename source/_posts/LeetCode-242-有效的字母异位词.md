---
title: LeetCode 242.有效的字母异位词
categories:
  - LeetCode
tags:
  - 哈希
abbrlink: 24a74496
date: 2023-04-17 21:20:02
---

# **[242.有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)**

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230417210119.png)

## Sample

### Input

```cpp
s = "anagram", t = "nagaram"
```

### Output

```cpp
true
```

### 提示

- `1 <= s.length, t.length <= 5 * 1e4`
- `s` 和 `t` 仅包含小写字母

## 思路

如果两个字符串互为字母异位词，那么这两个字母通过相同的排序条件排序之后，就是同一个字符串。因此我们可以判断这两个字符串排序完之后是否相同即可。但是排序的时间复杂度为$O(nlogn)$.时间复杂度不是很优秀，因此我们可以换一种思路。

既然这两个字符串排序完之后相等，那么这两个字符串每一个字母的个数都相同，因此我们只需要统计每个字符串的每种字母个数，如果两个字符串每种字母的个数都相同，那么这两个字符串就像等，否则就不是互为字母异位词。这个的时间复杂度为$O(n)$

## 做法

我们分别用两个数组来存储每个字符串的每种字母的个数，然后遍历两个数组，如果对应索引的字母个数都相同，那么这连个字符串互为字母异位词。

## 代码

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        int a[26] = {0}, b[26] = {0};
        for(auto ss : s)    a[ss - 'a']++;
        for(auto tt : t)    b[tt - 'a']++;
        for(int i = 0; i < 26; i++)
            if(a[i] != b[i]) return false;
        return true;
    }
};
```