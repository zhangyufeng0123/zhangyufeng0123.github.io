---
title: LeetCode 870. 优势洗牌
categories:
  - LeetCode
tags:
  - 每日一题
abbrlink: 672638a9
date: 2023-03-20 12:16:24
---

# [870. 优势洗牌](https://leetcode.cn/problems/advantage-shuffle/) (20221008每日一题)

## 题面
![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/870.png)

给定两个大小相等的数组 nums1 和 nums2，nums1 相对于 nums 的优势可以用满足 nums1[i] > nums2[i] 的索引 i 的数目来描述。

返回 nums1的**任意**排列，使其相对于 `nums2`的优势最大化。

## 思路

我们重排序`nums1`数组里的数字，使得`nums1`与`nums2`相同下标下`nums1`中的数比`nums2`中的数大，我们要求的是最大化情况下`nums1`数组中数字的排列情况。

这道题类似于田忌赛马，田忌赛马中有下等马、中等马、上等马，我们用我方的中等马配对对方的下等马，用上等马配对对方的中等马，用下等马配对对方的上等马。

在这里我们可以把数字看作马匹的好坏，我们需要用`nums1`中的较好的马匹配`nums2`中较差的马，用`nums1`中较差的马匹配nums2中最好的马。

## 做法

首先我们需要先排序，将`nums1`和`nums2`分别进行从小到大的顺序进行排序，其中`nums2`排序之后我们要记录每个数原来的索引值。这样方便将`nums1`的数放到对应的位置。

我们依次从`nums1`和`nums2`拿出`n1`和`n2`

- 如果`n1>n2`，那么`n1`就相当于上等马，`n2`相等于中等马或者下等马，这个时候我们就在`n2`对应的索引值上放入`n1`的值。
- 如果`n1≤n2`，那么`n1`就相当于下等马，我们将其与`nums2`中的最优秀的马进行对比。

## 代码

```cpp
class Solution {
public:
    static bool cmp(pair<int, int> &A, pair<int, int> &B){
        return A.first < B.first;
    }

    vector<int> advantageCount(vector<int>& nums1, vector<int>& nums2) {
        int len = nums1.size();
        vector<pair<int, int> > arr(len);
        vector<int> res(len);
        for(int i = 0; i < len; i++){
            arr[i] = pair<int, int> {nums2[i], i};
        }
        sort(nums1.begin(), nums1.end());
        sort(arr.begin(), arr.end(), this->cmp);

        int idx = 0, index = len - 1;
        for(int i = 0; i < len; i++){
            if(nums1[i] > arr[idx].first){
                res[arr[idx].second] = nums1[i];
                idx++;
            }else {
                res[arr[index].second] = nums1[i];
                index--;
            }
        }
        return res;
    }
};
```

## 结果

![Result](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/870-1.png)