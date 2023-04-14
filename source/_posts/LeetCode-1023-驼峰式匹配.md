---
title: LeetCode 1023.驼峰式匹配
categories:
  - LeetCode
tags:
  - 每日一题
abbrlink: 3effc393
date: 2023-04-14 11:04:52
---

# ****[1023.驼峰式匹配](https://leetcode.cn/problems/camelcase-matching/description/)****

## 题面

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230414105634.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230414105634.png)

## Sample

### Input

```cpp
queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FB"
```

### Output

```cpp
[true,false,true,true,false]
```

### Explain

```cpp
"FooBar" 可以这样生成："F" + "oo" + "B" + "ar"。
"FootBall" 可以这样生成："F" + "oot" + "B" + "all".
"FrameBuffer" 可以这样生成："F" + "rame" + "B" + "uffer".
```

### 数据范围

1. `1 <= queries.length <= 100`
2. `1 <= queries[i].length <= 100`
3. `1 <= pattern.length <= 100`
4. 所有字符串都仅由大写和小写英文字母组成。

## 思路

注意题目中，只能将小写字母插入模式串`pattern`，如果是大写的，那么说明不能匹配，返回`false`。那我们需要先扫一遍比较两个字符串的大写字母是否相同吗？答案是不需要，我们只要扫一遍就好了。

## 做法

我们将`queries`中的字符串与`pattern`进行一一比较，如果两个字符串对应的索引的字符相同，那么`pattern`的索引就`+1`，如果遇到字符不相同且`queries[i]`上的字符是大写字母时，那就返回`false`。注意，还有一种情况也是要返回`false`，即扫完一边`queries[i]`之后，我们的`pattern`字符串没有扫到最后一位，那么说明不管怎么在`pattern`中插入字符，都不可能变成`queries[i]`。

## 代码

```cpp
class Solution {
public:
    vector<bool> camelMatch(vector<string>& queries, string pattern) {
        int len = queries.size();
        vector<bool> res(len);
        for(int i = 0; i < len; i++){
            int idx = 0;
            bool flag = true;
            for(int j = 0; j < queries[i].size(); j++){
                if(idx < pattern.size() && queries[i][j] == pattern[idx]) ++idx;
                else if(queries[i][j] <= 'Z') flag = false;
            }
            if(idx != pattern.size()) flag = false;
            res[i] = flag;
        }
        return res;
    }
};
```