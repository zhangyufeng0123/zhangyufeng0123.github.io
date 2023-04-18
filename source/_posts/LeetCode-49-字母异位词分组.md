---
title: LeetCode 49.字母异位词分组
categories:
  - LeetCode
tags:
  - 哈希
abbrlink: 9170754a
date: 2023-04-18 22:51:45
---

# **[49.字母异位词分组](https://leetcode.cn/problems/group-anagrams/description/)**

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230418223356.png)

## Sample

### Input

```cpp
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
```

### Output

```cpp
[["bat"],["nat","tan"],["ate","eat","tea"]]
```

### 数据范围

- `1 <= strs.length <= 1e4`
- `0 <= strs[i].length <= 100`
- `strs[i]` 仅包含小写字母

## 思路

首先，我们得知道什么是字母异位词。两个字符串互为字母异位词当且仅当这两个字符串中所有的相同的字母的个数相等。

之前我们用判断两个字符串是否是互为字母异位词可以用判断相同字母个数是否相等来做，但是当涉及到多个字符串分类的时候，这个方法就不是那么方便了。换一种想法，两个字符串互为字母异位词，那么这两个字符串根据相同的优先级对字符进行排序之后就是相同的字符串了，因此我们可以对所有的字符串进行从小到大排序，然后记录每一种排完序之后的字符串的组别，最后我们将相同的字符串进行归类即可。

## 做法

用哈希表来判断每一种排完序之后的字符串是否已经出现过，如果出现过，那么我们就将这个字符串归入到相应的类别中，如果未出现过，那么我们就增加一个类别，并将这个字符串压入这个类别。

## 代码

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, int> hash;
        int len = strs.size();
        vector<int> index(len);
        int idx = 0;
        for(int i = 0; i < len; i++){
            string tmp = strs[i];
            sort(tmp.begin(), tmp.end());
            if(hash[tmp]) index[i] = hash[tmp];
            else {
                index[i] = ++idx;
                hash[tmp] = idx;
            }
        }
        vector<vector<string>> ans(idx);
        for(int i = 0; i < len; i++){
            ans[index[i] - 1].emplace_back(strs[i]);
        }
        return ans;
    }
};
```