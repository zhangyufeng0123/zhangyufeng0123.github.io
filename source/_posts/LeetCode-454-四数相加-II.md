---
title: LeetCode 454.四数相加 II
categories:
  - LeetCode
tags:
  - 哈希
abbrlink: 345aa0d2
date: 2023-04-18 22:06:52
---

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230418212548.png)

## Sample

### Input

```cpp
nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
```

### Output

```cpp
2
```

### Explain

```cpp
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

### 数据范围

- `n == nums1.length`
- `n == nums2.length`
- `n == nums3.length`
- `n == nums4.length`
- `1 <= n <= 200`
- `-2^28 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 2^28`

## 思路

刚开始的时候我想着这个数据真小，才`200`，因此我打算直接比完全暴力又那么不暴力一点的做法，用哈希存储前三个数组能够组成的和的个数，最后再遍历第四个数组即可。但是这样没想到超时了！后来一想，这样的时间复杂度是$O(n^3)$，这里有四个数组，完全可以用更小的时间复杂度来做。我们可以将四个数组两两分组，时间复杂度为$O(n^2)$，这样的话我们只要进行两次的两个数组之间的一一对应即可。

## 做法

我们用hash表来存储第一和第二两个数组每一种组合的和的个数，再组合另外两个数组的元素，每组合一次，就在hash表中查看是否有这个和的相反数，如果有，则加入这个相反数的二元组的个数。

## 代码

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> hash;
        int ans = 0;
        for(auto num1 : nums1){
            for(auto num2 : nums2){
                hash[num1 + num2]++;
            }
        }
        for(auto num1 : nums3){
            for(auto num2 : nums4){
                ans += hash[-num1 - num2];
            }
        }
        return ans;
    }
};
```