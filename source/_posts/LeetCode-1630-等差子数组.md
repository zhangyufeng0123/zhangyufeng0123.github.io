---
title: LeetCode 1630.等差子数组
categories:
  - LeetCode
tags:
  - 每日一题
  - 模拟
abbrlink: fdb0b5f8
date: 2023-03-23 12:53:51
---

## **[1630. 等差子数组](https://leetcode.cn/problems/arithmetic-subarrays/description/)**

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230323124209.png)

## Sample

### Input

```java
nums = [4,6,5,9,3,7], l = [0,0,2], r = [2,3,5]
```

### Output

```java
[true,false,true]
```

### Explain

```java
[true,false,true]
```

### 数据范围

- `n == nums.length`
- `m == l.length`
- `m == r.length`
- `2 <= n <= 500`
- `1 <= m <= 500`
- `0 <= l[i] < r[i] < n`
- `105 <= nums[i] <= 105`

## 思路

首先看一下难度，Medium，可以尝试着用暴力，但是要想一下怎么暴力。

单纯的暴力就是，将每一段区间的数都提出来再进行排序，再验证排完序之后的数组是否是等差数列。但是这样的时间复杂度是 $O(n^2logn)$，数据范围小的情况下可以做。

我有一个时间复杂度较小的想法。对每一段区间的步骤如下：

1. 找到该段区间的最大值和最小值，如果最大值和最小值之差不是个数`-1`的倍数，那么说明不能构成等差数列
2. 如果最大值和最小值之差刚好是0，那么说明这个区间中的所有数都相同，那么肯定是等差数列
3. 如果不是0，我们就要开始遍历这个区间的数
    1. 如果该区间的数与最小值之差不是公差的倍数，那说明这个区间的数不能构成等差数列
    2. 如果该区间重复出现某个数，说明这个区间的数不能构成等差数列

在经历了上述步骤之后，最后我们就能判断每个区间的数是否能构成等差数列了。

## 代码

```cpp
class Solution {
public:
    vector<bool> checkArithmeticSubarrays(vector<int>& nums, vector<int>& l, vector<int>& r) {
        int len_nums = nums.size(), len = l.size();
        vector<bool> ans(len);
        for(int i = 0; i < len; i++){
            int num_max = nums[l[i]], num_min = nums[l[i]];
            for(int j = l[i]; j <= r[i]; j++){
                num_max = max(num_max, nums[j]);
                num_min = min(num_min, nums[j]);
            }
            if((num_max - num_min) % (r[i] - l[i]) != 0) {
                ans[i] = false;
                continue;
            }
            unordered_map<int, int> hash;
            int e = (num_max - num_min) / (r[i] - l[i]);
            bool flag = true;
            if(e == 0){
                ans[i] = true;
                continue;
            }
            for(int j = l[i]; j <= r[i]; j++){
                if(hash[nums[j]] || (nums[j] - num_min) % e != 0){
                    flag = false;
                    break;
                }else{
                    hash[nums[j]] = 1;
                }
            }
            ans[i] = flag;
        }
        return ans;
    }
};
```