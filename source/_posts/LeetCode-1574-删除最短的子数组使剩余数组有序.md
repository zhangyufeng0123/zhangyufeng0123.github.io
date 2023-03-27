---
title: LeetCode 1574.删除最短的子数组使剩余数组有序
categories:
  - LeetCode
tags:
  - 双指针
  - 每日一题
abbrlink: 54f14d01
date: 2023-03-25 13:41:05
---

**[1574.删除最短的子数组使剩余数组有序](https://leetcode.cn/problems/shortest-subarray-to-be-removed-to-make-array-sorted/description/)**

## 题面

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230325131830.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230325131830.png)

## Sample

### Input

```cpp
arr = [1,2,3,10,4,2,3,5]
```

### Output

```cpp
3
```

### Explain

```cpp
我们需要删除的最短子数组是 [10,4,2] ，长度为 3 。剩余元素形成非递减数组 [1,2,3,3,5] 。
另一个正确的解为删除子数组 [3,10,4] 。
```

### 范围

- `1 <= arr.length <= 10^5`
- `0 <= arr[i] <= 10^9`

## 思路

题目中也说了，要删除的是连续子数组，不然可以用最长上升子数组的做法来做。

删除子数组不外乎有三种删法能使得剩下的数组是有序的。

- 删除前面的

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230325132852.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230325132852.png)

- 删除中间的

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230325132937.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230325132937.png)

- 删除后面的

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230325133020.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230325133020.png)

只有这三种删法才能使得剩下的是有序的。

那怎么删呢？

我的思路是用到双指针，且先找好右边的有序数组，然后开始枚举左边的数，如果左边的数是有序的，那么我们就根据左边的数来寻找右边相应的索引。这样的时间复杂度是$O(n)$。

## 做法

1. 首先我们得找先找右边最长的有序数组，并标记这个有序数组的起始索引`j`
2. 如果`j=0`，那么说明这个数组本就是有序数组，不用删减
3. 如果`j≠0`，那么我们就得从头遍历这个数组
4. 在我们遍历的过程中如果发现前面的数组不是有序的，那么立马停止遍历，因为继续遍历也没有用处，已经是无序的再怎么删也还是无序的
5. 我们每遍历一个数`num`就把这个数和刚刚我们标记的索引的值进行比较，如果`num`较小或相等，那么我们可以将之前最优解和当前最优解进行比较，当前最优解就是`j-num的索引-1`
6. 如果`num`较大，那么我们就将`j`往后推，直到`num`较小或者`j`推到末尾，这时再进行计算当前最优解

## 代码

```cpp
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int len = arr.size(), j = len - 1;
        while(j && arr[j -  1] <= arr[j]) j--;
        if(j == 0) return 0;
        int res = j;
        for(int i = 0; i < j; i++){
            if(i && arr[i] < arr[i - 1]) {
                res = min(res, len - i);
                break;
            }
            if(j != len && arr[i] <= arr[j]){
                res = min(res, j - i - 1);
            }else{
                while(j < len && arr[i] > arr[j]) j++;
                res = min(res , j - i - 1);
            }
        }
        return res;
    }
};
```