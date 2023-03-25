---
title: "LeetCode 2389.\_和有限的最长子序列"
categories:
  - LeetCode
tags:
  - 每日一题
  - 二分查找
  - 前缀和
abbrlink: 25b2a3da
date: 2023-03-17 20:26:57
---
# LeetCode 2389. 和有限的最长子序列

# **[2389. 和有限的最长子序列](https://leetcode.cn/problems/longest-subsequence-with-limited-sum/description/)**

## 题面

![Problem Description](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/2.png)
## Sample

### Input

```sql
nums = [4,5,2,1], queries = [3,10,21]
```

### Output

```sql
[2,3,4]
```

### Explain

```
- 子序列 [2,1] 的和小于或等于 3 。可以证明满足题目要求的子序列的最大长度是 2 ，所以 answer[0] = 2 。
- 子序列 [4,5,1] 的和小于或等于 10 。可以证明满足题目要求的子序列的最大长度是 3 ，所以 answer[1] = 3 。
- 子序列 [4,5,2,1] 的和小于或等于 21 。可以证明满足题目要求的子序列的最大长度是 4 ，所以 answer[2] = 4 。
```

数据范围是

- `n == nums.length`
- `m == queries.length`
- `1 <= n, m <= 1000`
- `1 <= nums[i], queries[i] <= 1e6`

## 思路

我们注意的第一点就是他给我们划重点的地方，我们要求的是子序列的最大长度

- 子序列
    
    是由一个数组删除某些元素（也可以不删除）但不改变剩余元素顺序得到的一个数组
    

题目让我们求子序列的最大长度，刚开始我以为是子串，需要连续，如果需要连续的话这道题的难度就会成倍地增长。如果是子序列那难度就会大大降低。因为我们是求子序列的总和，总和不用参考数组的顺序，那么我们可以随意的往子序列（最开始为空）中加入数字，直到这个总和大于`query`。

那么我们应该怎么加才能让这个子序列数组`size`最大呢？肯定是优先往里面加入最小的数，比如说现在的`nums`是`[4,5,2,1]`，`query`为`4`。如果我们往子序列中加入`4`，总和已经达到了要求，这个时候的`size`是`1`。但是如果我们往子序列中添加`1`，子序列的总和为`1`，还可以继续往里面添加，那么我们再选剩下的最小数`2`，这个时候子序列的总和为`3`，继续添加剩下的最小数`4`，总和达到了`7`，超过了`query`，那么我们就回退，`size`为`2`。这个例子我们就可以得出可以优先往里面添加最小数才能使得目标子序列的`size`最大。

## 做法

为了让添加最小数，我们可以先对数组进行排序，这样我们就能识别挑完最小数之后剩下的数中的最小数。

```cpp
class Solution {
public:
    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) {
        sort(nums.begin(), nums.end());
        vector<int> res(queries.size());
        int len = queries.size();
        for(int i = 0; i < len; i++){
            int t = 0;
            for(; t < nums.size(); t++){
                queries[i] -= nums[t];
                if(queries[i] < 0) break;
            }
            res[i] = t;
        }
        return res;
    }
};
```

为了避免我们对每个query都要进行一次累加，我们可以用前缀和的方式统计前`n`个较小数的总和，方便后面的寻找目标值。这样我们的时间复杂度就从 $O(m^2 * n)$降低到了 $O(m * n)$。

还可以进一步优化

因为我们现在已经获得了前缀和，还是一个排序后的数组的前缀和，那么这个前缀和数组也是一个有序数组。那么我们可以在这个有序数组中找到第一个`>query`的值的索引，这样我们就把时间复杂度从 $O(m*n)$降到了 $O(nlogm)$。

```cpp
class Solution {
public:
    int Binary_Search(vector<int> &nums, int target){
        int left = -1, right = nums.size();
        while(left + 1 < right){
            int mid = (left + right) >> 1;
            if(nums[mid] > target){
                right = mid;
            }else{
                left = mid;
            }
        }
        return left;
    }

    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) {
        sort(nums.begin(), nums.end());
        int len_nums = nums.size(), len_queries = queries.size();
        vector<int> qzh(len_nums, 0), ans(len_queries, 0);
        qzh[0] = nums[0];
        for(int i = 1; i < nums.size(); i++){
            qzh[i] = qzh[i - 1] + nums[i];
        }
        for(int i = 0; i < len_queries; i++){
            ans[i] = Binary_Search(qzh, queries[i]);
            ans[i]++;
        }
        return ans;
    }
};
```