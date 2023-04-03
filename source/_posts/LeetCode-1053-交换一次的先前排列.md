---
title: LeetCode 1053.交换一次的先前排列
categories:
  - LeetCode
tags:
  - 每日一题
  - 模拟
abbrlink: bee70a74
date: 2023-04-03 18:32:08
---

## 题面

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230403182351.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230403182351.png)

## Sample

### Input

```cpp
arr = [1,1,5]
```

### Output

```cpp
[1,1,5]
```

### Explain

```cpp
已经是最小排列
```

### 提示

- `1 <= arr.length <= 1e4`
- `1 <= arr[i] <= 1e4`

## 思路

按照题目的意思，就是找到该数组的字典序的上一个，如果这个数组已经是从小到大排列的有序数组，那么说明已经是最小了，那么就没有比这个更小的数，因此返回原数组，那么其它情况呢？我们只要从后往前找，每找一个数num的时候，就往后遍历，往后遍历的时候我们找到替换的数的要求有以下几点：

1. 数字比num小
2. 该数比其它小于num的数要大
3. 如果有多个相同的数，选择最近的那个

如果有满足以上几点的数，那就将这个数与num进行交换。

## 代码

```cpp
class Solution {
public:
    vector<int> prevPermOpt1(vector<int>& arr) {
        int len = arr.size();
        for(int i = len - 2; i >= 0; i--){
            int flag = -1, tmp = arr[i];
            for(int j = i + 1; j < len; j++){
                if(tmp > arr[j] && flag == -1){
                    flag = j;
                    tmp = arr[j];
                }else if(tmp < arr[j] && arr[i] > arr[j]){
                    flag = j;
                    tmp = arr[j];
                }
            }
            if(flag != -1){
                swap(arr[i], arr[flag]);
                return arr;
            }
        }
        return arr;
    }
};
```