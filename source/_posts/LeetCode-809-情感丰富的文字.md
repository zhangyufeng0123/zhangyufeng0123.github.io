---
title: "LeetCode 809.\_情感丰富的文字"
categories:
  - LeetCode
tags:
  - 每日一题
abbrlink: c10480a9
date: 2023-03-20 14:54:54
---
# [809. 情感丰富的文字](https://leetcode.cn/problems/expressive-words/description/)

# 题面

有时候人们会用重复写一些字母来表示额外的感受，比如 `"hello" -> "heeellooo"`, `"hi" -> "hiii"`。我们将相邻字母都相同的一串字符定义为相同字母组，例如："h", "eee", "ll", "ooo"。对于一个给定的字符串 S ，如果另一个单词能够通过将一些字母组扩张从而使其和 S 相同，我们将这个单词定义为可扩张的（stretchy）。扩张操作定义如下：选择一个字母组（包含字母 `c` ），然后往其中添加相同的字母 `c` 使其长度达到 3 或以上。

例如，以 "hello" 为例，我们可以对字母组 "o" 扩张得到 "hellooo"，但是无法以同样的方法得到 "helloo" 因为字母组 "oo" 长度小于 3。此外，我们可以进行另一种扩张 "ll" -> "lllll" 以获得 "helllllooo"。如果 `s = "helllllooo"`，那么查询词 "hello" 是可扩张的，因为可以对它执行这两种扩张操作使得 `query = "hello" -> "hellooo" -> "helllllooo" = s`。

输入一组查询单词，输出其中可扩张的单词数量。

# Sample

## Input

```cpp
s = "heeellooo"
words = ["hello", "hi", "helo"]
```

## Output

```cpp
1
```

# 思路

第一想法是利用双指针，但是双指针在这题代码写着比较麻烦我就放弃了，之后就打算用暴力的方法。首先我先记录字符串`s`中的连续字符以及个数，然后分别记录words中每个字符串中的连续字符以及个数，把他们分别存储到各自的数组中，如果两个数组的长度不一样，说明这words中的字符串不能扩充为字符串`s`。那么当数组长度一样时，我们就比较相同索引下的字符是否相等以及字符个数的比较。

如果字符不相等，说明该字符串不能扩充为`s`。假设`s`为`”aaabbb”`，这个时候存储`s`的数组为`[{’a’, 3}, {’b’, 3}]`，`words`中存在一个字符串为`”aaaccc”`，那么存储该字符串的数组为`[{’a’, 3}, {’c’, 3}]`。虽然两个字符串对应的数组相同，但是第二个字符串的第二个字符为`’c’`，不能变为`’b’`，因此该字符串不能转变为`s`。

再者，题目中有对扩张的要求。扩张的操作：选择一个字幕组（包含字母`c`），然后往其中添加相同的字母`c`使其长度达到3或以上。那么我们就可以的出，`”o”`不能转换成`”oo”`，因为`”oo”`的字符串长度小于3。但是`”oo”`可以转变成`”ooo"`，因为`“ooo”`的长度等于3，这符合扩张的操作。因此我们可以得出如果`s`中相同的字符的长度大于3，并且这个长度大于`words`对应字符串对应索引的字符长度，那么就可以被扩张，反之，有一个特例，即上述提到的`”o”`和`”oo”`之间的转化。

# 操作

首先我们对`s`进行操作，即用数组记录每一个连续字符的字符以及他的长度。之后我们对`words`中的每一个字符串进行相同的操作，然后比较两个数组。如果符合上述要求，那么`words`中的字符串就可以扩充为`s`，反之，不能。

# 代码

```cpp
class Solution {
public:
    int expressiveWords(string s, vector<string>& words) {
        int res = 0;
        vector<pair<char, int> > str, tmp;
        for(int i = 0; i < s.size(); i++){
            if(str.empty()) {
                str.emplace_back(pair<char, int> {s[i], 1});
            }else if(s[i] == str.back().first){
                str.back().second++;
            }else{
                str.emplace_back(pair<char, int> {s[i], 1});
            }
        }

        for(auto word : words){
            tmp.clear();
            for(int i = 0; i < word.size(); i++){
                if(tmp.empty() || word[i] != tmp.back().first){
                    tmp.emplace_back(pair<char, int> {word[i], 1});
                }else {
                    tmp.back().second++;
                }
            }

            if(tmp.size() != str.size()) continue;
            int flag = 0;
            for(int i = 0; i < str.size(); i++){
                if(str[i].first != tmp[i].first || ((str[i].second - tmp[i].second) == 1 && str[i].second < 3) || str[i].second - tmp[i].second < 0) {
                    flag = 1;
                    break;
                }
            }
            if(!flag) {
                res++;
            }
        }
        return res;
    }
};
```