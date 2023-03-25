---
title: LeetCode 209.长度最小的子数组
categories:
  - LeetCode
tags:
  - 双指针
abbrlink: 598a88dc
date: 2023-03-20 14:49:10
---
# [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/description/)

## 题面

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/209.png)

## Sample1

### Input:

```cpp
target = 7, nums = [2,3,1,2,4,3]
```

### Output:

```cpp
2
```

## Sample2

### Input:

```cpp
target = 4, nums = [1,4,4]
```

### Output:

```cpp
1
```

## 思路

看到这个题目的数组长度范围是`[1,1e5]`，如果使用暴力的方法，也就是遍历所有的子数组，此时的时间复杂度是 $O(n^2)$，这个时候是有可能超时的，那么我们怎么来降低耗时呢？这个时候我们就可以利用滑动窗口的思路，即先记录一段子数组的总和，如果总和大于`target`，那么我们就可以慢慢减去这个子数组的最前面的数字，直到子数组的和小于`target`，在减的过程中我们统计这个子数组的长度与当前最优解进行比较，如果子数组的长度优于当前最优解时，我们就替换当前最优解。

## 做法

我们先用一个`idx`变量来记录这个数组的左端，然后遍历数组。在遍历数组的时候，我们用一个变量`sum`来记录从`idx`到`i`子数组总和。当这个总和`sum`的值大于等于`target`时，我们就慢慢删减`idx`上的数，同时把`idx`后移一位，直到`sum`小于`target`，在删减的同时更新最优解。

## 代码

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int res = nums.size() + 1;
        int idx = -1, sum = 0;
        for(int i = 0; i < nums.size(); i++){
            sum += nums[i];
            while(sum >= target){
                res = min(i - idx, res);
                idx++;
                sum -= nums[idx];
            }
        }
        if(res == nums.size() + 1) return 0;
        return res;
    }
};
```