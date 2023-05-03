---
title: LeetCode 1003.检查替换后的词是否有效
abbrlink: 5afd8342
date: 2023-05-03 13:48:41
categories:
tags:
---
## **[1003.检查替换后的词是否有效](https://leetcode.cn/problems/check-if-word-is-valid-after-substitutions/description/)**

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230503132845.png)

## Sample

### Input

```cpp
s = "aabcbc"
```

### Output

```cpp
true
```

### Explain

```cpp
"" -> "abc" -> "aabcbc"
因此，"aabcbc" 有效。
```

### 数据范围

- `1 <= s.length <= 2 * 1e4`
- `s` 由字母 `'a'`、`'b'` 和 `'c'` 组成

## 思路

这道题其实是和括号匹配是类似的，只不过现在是由`()`变成了`”abc”`。我们可以想想如果是`()`的话应该怎么解决。通常的做法都是使用栈来做，我们每碰见一个`(`时，我们就将左括号压入堆栈中，直到遇到`)`才取出栈顶的左括号进行匹配。这里也是一样，我们遇见`’a’`时将字符压入堆栈中，遇到字符`’b’`时查看栈顶是否为`’a’`，如果不为`’a’`，那么说明字符串无法匹配。相同的时，如果我们遇到字符`’c’`时，我们就得查看前两个字符是`”ab”`，如果不是则无法匹配。

## 代码

代码有两个版本，第一个版本正确省事，但是剪枝效果并不怎么好，因为如果我们遇到`b`时没有判断这个`b`是否合法。这个版本的耗时时12ms

```cpp
class Solution {
public:
    bool isValid(string s) {
        string str;
        for(int i = 0; i < s.size(); i++){
            if(s[i] == 'c'){
                if(str.size() < 2) return false;
                if(str.substr(str.size() - 2, 2) != "ab") return false;
                else {
                    str.pop_back();
                    str.pop_back();
                }
            }else{
                str += s[i];
            }
        }
        if(str.empty()) return true;
        return false;
    }
};
```

这个版本我们验证字符`b`是否合法，在这一基础上剪枝效果显著，耗时优化到了8ms

```cpp
class Solution {
public:
    bool isValid(string s) {
        string str;
        for(int i = 0; i < s.size(); i++){
            if(s[i] == 'c'){
                if(str.size() < 2) return false;
                if(str.substr(str.size() - 2, 2) != "ab") return false;
                else {
                    str.pop_back();
                    str.pop_back();
                }
            }else if(s[i] == 'b'){
                if(str.empty()) return false;
                if(str.back() != 'a') return false;
                str += 'b';
            }else str += 'a';
        }
        if(str.empty()) return true;
        return false;
    }
};
```