---
title: LeetCode 811. 子域名访问计数
categories:
  - LeetCode
tags:
  - 每日一题
  - Hash
abbrlink: 57efb66a
date: 2023-03-20 12:03:59
---
# LeetCode 811. 子域名访问计数

# ****[811. 子域名访问计数](https://leetcode.cn/problems/subdomain-visit-count/)(20221005每日一题)**

![ProblemDescription](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/811.png)

网站域名 "[discuss.leetcode.com](http://discuss.leetcode.com/)" 由多个子域名组成。顶级域名为 "com" ，二级域名为 "[leetcode.com](http://leetcode.com/)" ，最低一级为 "[discuss.leetcode.com](http://discuss.leetcode.com/)"。当访问域名 `"discuss.leetcode.com"`时，同时也会隐式访问其父域名 `"leetcode.com"` 以及 `"com"`。

计数配对域名 是遵循 "rep d1.d2.d3" 或 "rep d1.d2" 格式的一个域名表示，其中 rep 表示访问域名的次数，d1.d2.d3 为域名本身。

例如，"9001 [discuss.leetcode.com](http://discuss.leetcode.com/)" 就是一个 计数配对域名 ，表示 [discuss.leetcode.com](http://discuss.leetcode.com/) 被访问了 9001 次。

给你一个 **计数配对域名** 组成的数组 `cpdomains` ，解析得到输入中每个子域名对应的 **计数配对域名**，并以数组形式返回。可以按 **任意顺序**返回答案。

## 思路

我们需要获得所有的域名以及他们的子域名，这个时候我们最好的想法就是用一个hash来存储所有出现过的域名，如果用其他方法不太现实。用hash来存储时我们不仅要考虑到出现的域名，我们还要考虑到这些域名出现的次数。

## 做法

对于每个字符串，首先，我们先正序遍历，找到这个网站出现的个数，然后再逆序遍历找到所有的子域名，找完所有的子域名之后记录每个子域名在当前情况下访问了几次，然后通过hash表进行累加。最后我们遍历hash表获取所有的域名以及他们的访问次数。

## 代码

```jsx
class Solution {
public:
    vector<string> subdomainVisits(vector<string>& cpdomains) {
        unordered_map<string, int> hash;
        for(auto c : cpdomains){
            int i = 0;
            string num;
						//统计访问次数
            for(; i < c.size(); i++){
                if(c[i] == ' ') break;
                num += c[i];
            }
            int times = stoi(num);
            string com;
						//统计域名
            for(int j = c.size() - 1; j > i; j--){
                if(c[j] == '.'){
                    hash[com] += times;
                }
                com = c[j] + com;
            }
            hash[com] += times;
        }
        vector<string> res;
				//遍历hash表获取每个子域名以及访问次数
        for(auto ha : hash){
            string t = to_string(ha.second) + ' ' + ha.first;
            res.emplace_back(t);
        }

        return res;
    }
};
```