---
title: LeetCode 周赛339
categories:
  - LeetCode
tags:
  - 周赛
  - 贪心
  - 排序
abbrlink: fa30027a
date: 2023-04-03 18:20:30
---
# LeetCode 周赛339

# **[6362.最长平衡子字符串](https://leetcode.cn/problems/find-the-longest-balanced-substring-of-a-binary-string/)**

## 题面

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230402130249.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230402130249.png)

## Sample

### Input

```cpp
s = "01000111"
```

### Output

```cpp
6
```

### Explain

```cpp
最长的平衡子字符串是 "000111" ，长度为 6 。
```

## 思路

这道题刚开始没看清题意，以为1可以在0的前面以及可以混插而不是0和1分开，如果是这样的话，我们只要记录字符串中0的个数和1的个数，最后返回数量较小的两倍即可。

但是上面的Sample和我最开始的思路不同。所以我就仔细看了看题目。

题目中明确提出了，在平衡子字符串中，**所有的`0`都在`1`之前**。因此我就换了一种做法，把字符串分若干段，再综合每一段的最大值取最优解。

那么问题来了，怎么分段呢？根据上面的要求，所有的0都在1之前，那么只要出现1在0之前，就分一个段落。

分完段落求结果即可。

## 代码

```cpp
class Solution {
public:
    int findTheLongestBalancedSubstring(string s) {
        int res = 0, tmp0 = 0, tmp1 = 0;
        for(int i = 0; i < s.size(); i++){
            if(i != 0 && s[i] == '0' && s[i - 1] == '1'){
                res = max(res, min(tmp0, tmp1) * 2);
                tmp0 = 0;
                tmp1 = 0;
            }
            if(s[i] == '0') tmp0++;
            else tmp1++;
        }
        res = max(res, min(tmp0, tmp1) * 2);
        return res;
    }
};
```

# **6363.转换二维数组**

## 题面

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230402131000.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230402131000.png)

## Sample

### Input

```cpp
nums = [1,3,4,1,2,3,1]
```

### Output

```cpp
[[1,3,4,2],[1,3],[1]]
```

### Explain

```cpp
根据题目要求可以创建包含以下几行元素的二维数组：
- 1,3,4,2
- 1,3
- 1
nums 中的所有元素都有用到，并且每一行都由不同的整数组成，所以这是一个符合题目要求的答案。
可以证明无法创建少于三行且符合题目要求的二维数组。
```

### 范围

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= nums.length`

## 思路

这道题的数据量不到200，直接暴力贪心即可。

## 做法

我们先统计数组中每个数的个数，然后就开始循环遍历数组。

在每次循环中我们用一个一维数组来标记每个数是否已经被压入数组中，每压入一个数时，我们就在之前用来记录数组中每个数的个数的数组中的对应位置将其减1，如果是0，那么说明该数已经不能再被使用。最后如果记录个数的数组中所有值都是0，那么就停止循环。

## 代码

```cpp
class Solution {
public:
    vector<vector<int>> findMatrix(vector<int>& nums) {
        vector<vector<int>> res;
        unordered_map<int, int> hash;
        vector<int> tmp;
        for(auto num : nums) hash[num]++;
        int len = nums.size(), k = 0;
        while(1){
            vector<int> f(201, 0);
            for(int num : nums){
                if(hash[num] && !f[num]){
                    f[num] = 1;
                    hash[num]--;
                    tmp.emplace_back(num);
                }
            }
            if(!tmp.empty()){
                res.emplace_back(tmp);
            }else break;
            tmp.clear();
        }
        return res;
    }
};
```

# **[6364.老鼠和奶酪](https://leetcode.cn/problems/mice-and-cheese/)**

## 题面

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230402131723.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230402131723.png)

## Sample

### Input

```cpp
reward1 = [1,1,3,4], reward2 = [4,4,1,1], k = 2
```

### Output

```cpp
15
```

### Explain

```cpp
这个例子中，第一只老鼠吃掉第 2 和 3 块奶酪（下标从 0 开始），第二只老鼠吃掉第 0 和 1 块奶酪。
总得分为 4 + 4 + 3 + 4 = 15 。
15 是最高得分。
```

### 范围

- `1 <= n == reward1.length == reward2.length <= 1e5`
- `1 <= reward1[i], reward2[i] <= 1000`
- `0 <= k <= n`

## 思路

这道题的意思就是在老鼠1只能吃k个的情况下两只老鼠最多能吃多少个。

贪心，直接贪即可，那么怎么贪呢？

老鼠1只能吃k个，那我们肯定让老鼠1吃分高的，但是如果老鼠1吃的相对高的分老鼠2对应的分也高应该怎么办呢？我们就让老鼠1吃分相对于老鼠2的分较高的。那么怎么体现这个相对呢？很简单，我们只要做差即可，差值越大，那么说明老鼠1吃这个蛋糕得到的分就越划算。

## 做法

首先我们对每个蛋糕对应的老鼠1和老鼠2的得分进行做差，然后从大到小进行排序。要想使最后的分数最高，那么我们只要让老鼠1吃排完序后的前k个蛋糕即可。剩余的蛋糕老鼠2吃。

## 代码

```cpp
class Solution {
public:
    int miceAndCheese(vector<int>& reward1, vector<int>& reward2, int k) {
        int len = reward1.size();
        vector<pair<int, int>> nums(len);
        for(int i = 0; i < len; i++){
            nums[i] = pair<int, int> {i, reward1[i] - reward2[i]};
        }
        sort(nums.begin(), nums.end(), [&](pair<int, int> &A, pair<int, int> &B){
            return A.second > B.second;
        });
        int res = 0;
        for(int i = 0; i < len; i++){
            if(i < k) res += reward1[nums[i].first];
            else res += reward2[nums[i].first];
        }
        return res;
    }
};
```