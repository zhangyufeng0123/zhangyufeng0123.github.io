---
title: LeetCode 1616.分割两个字符串得到回文串
categories:
  - LeetCode
tags:
  - 每日一题
  - 贪心
abbrlink: b55ae9ab
date: 2023-03-20 15:09:40
---

# **[1616. 分割两个字符串得到回文串](https://leetcode.cn/problems/split-two-strings-to-make-palindrome/)(Medium)**

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/1616.png)

## Sample

### Input

```cpp
a = "ulacfd", b = "jizalu"
```

### Output

```cpp
true
```

### Explain

```cpp
aprefix = "ula", asuffix = "cfd"
bprefix = "jiz", bsuffix = "alu"
那么 aprefix + bsuffix = "ula" + "alu" = "ulaalu" 是回文串。
```

### 数据范围

- `1 <= a.length, b.length <= 105`
- `a.length == b.length`
- `a` 和 `b` 都只包含小写英文字母

## 思路

这道题最暴力的想法，就是现在每个地方切一刀获取两个新形成的字符串，然后再比较每一个字符串是否是回文串，这样的时间复杂度是 $O(n^2)$，耗时比较高。

怎么降低耗时呢？

我的做法就是去除冗余的比较，比如说样例给的两个字符串”ulacfd”,”jizalu”，划在第一刀时形成的字符串”uizalu”,”jlacfd”，和划在第二刀形成的字符串”ulzalu”,”jiacfd”，其中第一个字符串首字母”u“和第二个字符串末字母”u“对比了两次，但是他们在位置上是构成回文的。如果我们先把这些能够构成回文的位置都找到，那么我们就能够减少很多冗余的对比。再给一组例子，“ulbbfc”,”jizalu”，我们已经找到了首尾能够构成回文的字符串了，”ul”,”lu”，但是剩下的中间部分不能由两个字符串拼接而成了，如果能够继续拼接，那么说明获取能够构成回文的字符串应该是”ulb”，”alu”，这很明显是不能构成回文的。

那么接下来应该怎么做呢？

既然剩下的部分不能由两个字符串拼接而成，那么我们就在两个字符串剩下的部分来判断是否是回文串即可，比如“ulbbfc”,”jizalu”这组例子，我们已经获得了目标回文串的首尾”ul“和”lu”，这个首尾能够组成的字符串只能是”ulzalu”，”ulbalu”，”ulbblu”，但是因为中间部分不能由两个字符串拼接，那么只剩下两个字符串”ulzalu”和”ulbblu”。也就是说，我们只要判断”za””bb”是不是回文串即可。

## 做法

这道题因为是`a$_{prefix}$ + b$_{suffix}$`或者`b$_{prefix}$ + a$_{suffix}$`其中有一个满足就行，那么我们只需要分别做`a$_{prefix}$ + b$_{suffix}$`和`b$_{prefix}$ + a$_{suffix}$`即可。即先判断a字符串前和b字符串末能够构成回文的最大长度，然后在两个字符串剩余部分判断是否构成回文，如果最后的字符串不是回文，那么再用相同的方法判断另外一组。

## 代码

```cpp
class Solution {
public:
    bool chechAgain(string a, int i, int j){
        if(i >= j) return true;
        while(i < j){
            if(a[i] == a[j]) {
                i++;
                j--;
            }else break;
        }
        if(i < j) return false;
        return true;
    }

    bool check(string a, string b){
        int i = 0, j = a.size() - 1;
        while(i < j){
            if(a[i] == b[j]){
                i++;
                j--;
            }
            else break;
        }
        return chechAgain(a, i ,j) || chechAgain(b, i, j);
    }

    bool checkPalindromeFormation(string a, string b) {
        return check(a, b) || check(b, a);
    }
};
```