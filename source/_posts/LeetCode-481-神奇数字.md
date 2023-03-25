---
title: LeetCode 481.神奇数字
categories:
  - LeetCode
tags:
  - 每日一题
  - 模拟
abbrlink: '16343545'
date: 2023-03-20 14:43:09
---
# [481. 神奇字符串](https://leetcode.cn/problems/magical-string/)(20221031每日一题)

# 题面

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/481-1.png)
# Sample

## Input

```cpp
6
```

## Output

```cpp
3
```

# 思路

这道题给出一个一维数组，后面的数会根据前面的数而定值，那么我们只需要模拟一下即可，那么怎么模拟呢？

定义这个一维数组为`arr`，那么我们可以看到。`arr[2]`的数字要根据`arr[1]`来定值，`arr[3]`的数字要根据`arr[2]`来定值，因为`arr[0]`为`1`，`arr[1]`为`2`，而因为`arr[1]`为`2`，那么`arr[1]`和`arr[2]`需要相同的数字，即`arr[2] = arr[1]`，之后`arr[3]`的值不能与`arr[2]`相等，否则就成为了`arr[1]=arr[2]=arr[3]`，这个时候`arr[1]`的值需要为`3`才能解释的通，但是`arr`的值只能是`1`或者`2`，那么我们就要将改变`arr[3]`的值与前面的值不同。

因此我们需要定义一个索引值`idx`，来表示当前相同数字所需要的连贯个数，用一个变量`f`来表示当前数组的数字，每加入一个数字时，我们就对`arr[idx]`进行减一操作，当`arr[idx]`为`0`时，我们就需要对`idx`进行`++`操作，并且改变`f`的值，使得数字之间不相同。

# 代码

```cpp
class Solution {
public:
    int magicalString(int n) {
        if(n <= 3) return 1;
        vector<int> arr(n);
        arr[0] = 1;
        arr[1] = 2;
        arr[2] = 2;
        int ones = 1, idx = 2, f = 1;
        for(int i = 3; i < n; i++){
            if(f == 1) ones++;
            arr[i] = f;
            arr[idx]--;
            if(arr[idx] == 0) {
                f = 3 - f;
                idx++;
            }
        }
        return ones;
    }
};
```

# 结果

![Result](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/481-2.png)