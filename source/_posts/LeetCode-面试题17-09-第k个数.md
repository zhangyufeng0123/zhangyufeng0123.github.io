---
title: LeetCode 面试题17.09. 第k个数
categories:
  - LeetCode
tags:
  - 多指针
abbrlink: 478d69b2
date: 2023-03-20 11:57:50
---
# LeetCode 面试题17.09. 第k个数

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/1709.png)

这道题让我们从一个只包含质因子3，5，7数字的数组中，根据从小到大排序，获取第k个数。

## 思路

刚开始的想法，就是根据一个优先队列，让较小的数优先弹出，但是这种做法比较麻烦的就是你的优先队列需要维护的长度是k*3，而优先队列的复杂度是O(log n)，再结合需要经过3k次的插入以及多次弹出，复杂度不够理想。

后面看了一个大佬的做法，利用三指针，用三个指针p3,p5,p7分别记录上次乘以质因子3、5、7的数组index，然后比较num[p3] * 3, num[p5] * 5, num[p7] * 7三者的大小，谁最小就更新哪个质因子的指针。

```cpp
class Solution {
public:
    int getKthMagicNumber(int k) {
        int *nums = new int[k + 1];
        nums[0] = 1;
        int p3 = 0, p5 = 0, p7 = 0;
        for(int i = 1; i < k; i++){
            nums[i] = min(nums[p3] * 3, min(nums[p5] * 5, nums[p7] * 7));
            if(nums[i] == nums[p3] * 3) p3++;
            else if(nums[i] == nums[p5] * 5) p5++;
            else if(nums[i] == nums[p7] * 7) p7++;
        }
        return nums[k - 1];
    }
};
```

这里出现了一个问题，当时我没有考虑到去重，这边的顺序是1,3,5,7,9,15,15,21,21,….。会有重复的数字，我们在每次比较中，要将每个最小值的质因子的指针更新。

```cpp
class Solution {
public:
    int getKthMagicNumber(int k) {
        int *nums = new int[k + 1];
        nums[0] = 1;
        int p3 = 0, p5 = 0, p7 = 0;
        for(int i = 1; i < k; i++){
            nums[i] = min(nums[p3] * 3, min(nums[p5] * 5, nums[p7] * 7));
            if(nums[i] == nums[p3] * 3) p3++;
            if(nums[i] == nums[p5] * 5) p5++;
            if(nums[i] == nums[p7] * 7) p7++;
        }
        return nums[k - 1];
    }
};
```