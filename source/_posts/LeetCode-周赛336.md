---
title: LeetCode 周赛336
categories:
  - LeetCode
tags:
  - 周赛
  - 异或
  - 贪心
abbrlink: 6a8f1feb
date: 2023-03-20 15:04:18
---
# LeetCode周赛336

# **[6315. 统计范围内的元音字符串数](https://leetcode.cn/problems/count-the-number-of-vowel-strings-in-range/description/)(Easy)**

## 题面

给你一个下标从 **0** 开始的字符串数组 `words` 和两个整数：`left` 和 `right` 。

如果字符串以元音字母开头并以元音字母结尾，那么该字符串就是一个 **元音字符串** ，其中元音字母是 `'a'`、`'e'`、`'i'`、`'o'`、`'u'` 。

返回 **`words[i]` 是元音字符串的数目，其中 **`i` 在闭区间 `[left, right]` 内。

## Sample

### Input

```cpp
words = ["are","amy","u"], left = 0, right = 2
```

### Output

```cpp
2
```

## 范围

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 10`
- `words[i]` 仅由小写英文字母组成
- `0 <= left <= right < words.length`

## 思路

我们直接从下标为`left`的字符串遍历到下标为`right`的字符串，在遍历的时候我们需要检验每个字符串是否符合要求。

## 做法

我们先用哈希表来存储元音字母，然后检验每个字符串的开头字母和结尾字母是不是元音字母即可。

时间复杂度是 $O(n)$。

## 代码

```cpp
class Solution {
public:
    int vowelStrings(vector<string>& words, int left, int right) {
        int res = 0;
        unordered_map<char, int> hash;
        hash['a'] = 1;
        hash['e'] = 1;
        hash['i'] = 1;
        hash['o'] = 1;
        hash['u'] = 1;
        for(int i = left; i <= right; i++){
            if(hash[words[i][0]] && hash[words[i].back()]) res++;
        }
        return res;
    }
};
```

# **[6316. 重排数组以得到最大前缀分数](https://leetcode.cn/problems/rearrange-array-to-maximize-prefix-score/)(Medium)**

## 题面

给你一个下标从 **0** 开始的整数数组 `nums` 。你可以将 `nums` 中的元素按 **任意顺序** 重排（包括给定顺序）。

令 `prefix` 为一个数组，它包含了 `nums` 重新排列后的前缀和。换句话说，`prefix[i]` 是 `nums` 重新排列后下标从 `0` 到 `i` 的元素之和。`nums` 的 **分数** 是 `prefix` 数组中正整数的个数。

返回可以得到的最大分数

## Sample

### Input

```cpp
[2,-1,0,1,-3,3,-3]
```

### Output

```cpp
6
```

## 范围

- `1 <= nums.length <= 1e5`
- `-1e6 <= nums[i] <= 1e6`

## 思路

这道题让我们求前`n`个数使得这`n`个数的和大于`0`，要使得`n`最大，那么我们要保证这个和是整数，那么我们肯定希望这`n`个数里面的正数愈多越好，因此我们优先把正数放在前面。如果没有正数了，有`0`肯定优先加入`0`，这样不会影响这`n`个数之和的正负性，之后才添加负数，那么怎么添加呢？肯定不是添加最小的负数，因为你添加一个最小的负数说不定等价于添加`2`个甚至更多的负数，因此我们优先添加最大的负数，直到和变为非正数。

## 做法

对数组从大到小进行排序，然后依次遍历累加直到和为非正数。注意到`nums[i]`的范围是`[-1e6,1e6]`，个数为`1e5`，所以我们要注意累加的和的变量类型，`1e11`的数是超了`int`的。

排序的时间复杂度为 $O(nlogn)$，累加的时间复杂度为 $O(n)$。因此总的时间复杂度为 $O(nlogn)$。

## 代码

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        sort(nums.begin(), nums.end(), [&](int &A, int &B){
            return A > B;
        });
        long long tmp = 0;
        int len = nums.size();
        for(int i = 0; i < len; i++){
            tmp += nums[i];
            if(tmp <= 0) return i;
        }
        return len;
    }
};
```

# **[6317. 统计美丽子数组数目](https://leetcode.cn/problems/count-the-number-of-beautiful-subarrays/)(Medium)**

## 题面

给你一个下标从 **0** 开始的整数数组`nums` 。每次操作中，你可以：

- 选择两个满足 `0 <= i, j < nums.length` 的不同下标 `i` 和 `j` 。
- 选择一个非负整数 `k` ，满足 `nums[i]` 和 `nums[j]` 在二进制下的第 `k` 位（下标编号从 **0** 开始）是 `1` 。
- 将 `nums[i]` 和 `nums[j]` 都减去 `2k` 。

如果一个子数组内执行上述操作若干次后，该子数组可以变成一个全为 `0` 的数组，那么我们称它是一个 **美丽** 的子数组。

请你返回数组 `nums` 中 **美丽子数组** 的数目。

子数组是一个数组中一段连续 **非空** 的元素序列。

## Sample

### Input

```cpp
[4,3,1,2,4]
```

### Output

```cpp
2
```

## 思路

题目的意思是让我们求美丽子数组的个数，美丽子数组中的数字在二进制每一位上都是偶数，比如`[3,1,2]`中，用二进制表示是`[11,1,10]`，二进制中每一位都有两个`[2,2]`，两个`2`分别代表有`2`个 $2^1$和2个 $2^0$。因为只有个数是偶数时我们才能在删减操作使得数组中的数都为`0`，如果数组是`[3,1,1]`，二进制表示是`[11,1,1]`，二进制中每一位的表示是`[1,3]`，表示有`1`个 $2^1$和`3`个 $2^0$，这样的数组进行删减操作之后还会剩下`1`个 $2^1$和`1`个 $2^0$，得不到完美数组。

相当于我们要使美丽数组中的数异或和为`0`.

- 异或
    
    异或的性质就是两个数相同时为`0`，不同时为`1`即
    
    0 ^ 0 = 0
    
    1 ^ 0 = 1
    
    0 ^ 1 = 1
    
    1 ^ 1 = 0
    

那么这道题就可以转换成有多少个子数组的异或和为`0`.

## 做法

这边我是参考了前缀和的思想。

先用一个数组来维护从头到当前位置所有数的异或和，比如用样例给的数组`[4,3,1,2,4]`，那么他的前缀异或和就是`[4,7,6,4,0]`。记录完之后我们就要开始统计每一个异或和他在前缀异或和中出现了多少次。

要求美丽子数组的话我们就需要统计后面出现多少次与当前相同的异或和。为什么要找到相同的呢？比如说上面给的样例，有两个相同的异或和`4`，第一个`4`的异或和是`nums[0]`，第二个异或和是`nums[0]^nums[1]^nums[2]^nums[3]`。相同的两个数字进行异或操作之后的结果是`0`，那么`nums[0]^(nums[0]^nums[1]^nums[2]^nums[3])`之和也是`0`，`nums[0]^(nums[0]^nums[1]^nums[2]^nums[3])=0`等价于`nums[0]^nums[0]^nums[1]^nums[2]^nums[3]=0`，因为相同的数异或之后等于`0`，那么`nums[0]`可以消除，因此上面的等式等价于`nums[1]^nums[2]^nums[3]=0`，这样就有了一个美丽子数组。

## 代码

```sql
class Solution {
public:
    long long beautifulSubarrays(vector<int>& nums) {
        int len = nums.size();
        vector<int> num(len, 0);
        num[0] = nums[0];
        for(int i = 1; i < len; i++){
            num[i] = num[i - 1] ^ nums[i];
        }
        unordered_map<int ,int> hash;//记录每个异或和出现的个数
        for(int i = 0; i < len; i++){
            hash[num[i]]++;
        }
        long long res = hash[0];
        for(int i = 0; i < len; i++){
            hash[num[i]]--;//进行删减操作是为了不让前面的异或和影响到后面的累加
            res += hash[num[i]];
        }
        return res;
    }
};
```

# **[2589. 完成所有任务的最少时间](https://leetcode.cn/problems/minimum-time-to-complete-all-tasks/)(Hard)**

## 题面

你有一台电脑，它可以 **同时** 运行无数个任务。给你一个二维整数数组 `tasks` ，其中 `tasks[i] = [starti, endi, durationi]` 表示第 `i` 个任务需要在 **闭区间** 时间段 `[starti, endi]` 内运行 `durationi` 个整数时间点（但不需要连续）。

当电脑需要运行任务时，你可以打开电脑，如果空闲时，你可以将电脑关闭。

请你返回完成所有任务的情况下，电脑最少需要运行多少秒。

## Sample

### Input

```sql
[[2,3,1],[4,5,1],[1,5,2]]
```

### Output

```sql
2
```

## 范围

- `1 <= tasks.length <= 2000`
- `tasks[i].length == 3`
- `1 <= starti, endi <= 2000`
- `1 <= durationi <= endi - starti + 1`

## 思路

参考灵神大佬说的话，像这种区间题一般都是根据右端点从小到大进行排序。

这道题在写的时候想到排序，但是没考虑到数据范围就没有莽上去A，不应该。看了直播回放之后自己写一次AC。

这道题主要是贪心的想法，我们根据排序之后区间从头开始遍历，我们每遍历一个区间的时候，我们得优先把CPU时间选在末尾，因为我们不能保障左端点之间能够有交集，但是我们能保证右端能够产生交集，而要减少CPU运行时间，交集的区间要越大越好，因此我们优先把CPU时间放在每个区间的右端。

## 做法

根据右端进行排序，然后依次遍历，我们遍历到每个区间时，先考虑这段区间之中有多少个区间已经有CPU在使用了，然后将还需要的时间划分到区间的右端，最后统计有多少时间使用CPU即可。

## 代码

```sql
class Solution {
public:
    int findMinimumTime(vector<vector<int>>& tasks) {
        sort(tasks.begin(), tasks.end(), [&](vector<int> &A, vector<int> &B){
            return A[1] < B[1];
        });
        vector<int> f(2001, 0);
        int len = tasks.size();
        int res = 0;
        for(int i = 0; i < len; i++){
            int tmp = 0;
            for(int j = tasks[i][0]; j <= tasks[i][1]; j++){
                tmp += f[j];
            }
            if(tmp >= tasks[i][2]) continue;
            tmp = tasks[i][2] - tmp;
            // cout << tmp << endl;
            for(int j = tasks[i][1]; j >= tasks[i][0]; j--){
                if(!f[j]){
                    f[j] = 1;
                    tmp--;
                    res++;
                    if(!tmp) break;
                }
            }
        }
        return res;
    }
};
```