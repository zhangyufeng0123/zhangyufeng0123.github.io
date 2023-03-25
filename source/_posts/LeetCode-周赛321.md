---
title: LeetCode 周赛321
categories:
  - LeetCode
tags:
  - 周赛
  - 双指针
  - 前缀和
abbrlink: edf0bb09
date: 2023-03-20 14:56:42
---
# LeetCode 周赛321

# **[6245. 找出中枢整数](https://leetcode.cn/problems/find-the-pivot-integer/)(Easy)**

## 题面

给你一个正整数 `n` ，找出满足下述条件的 **中枢整数** `x` ：

- `1` 和 `x` 之间的所有元素之和等于 `x` 和 `n` 之间所有元素之和。

返回中枢整数 **`x` 。如果不存在中枢整数，则返回 `-1` 。题目保证对于给定的输入，至多存在一个中枢整数。

## Sample

### Input:

```cpp
n = 8
```

### Output:

```cpp
6
```

### 范围

• `1 <= n <= 1000`

## 思路

作为周赛的前三题，直接暴力做即可，并且数据范围是`1`到`1000`，那么我们只要直接遍历一遍即可

## 做法

我们用两个变量来存储`[1,x]`的总和，和`[x,n]`的总和。

然后每向后移一位，我们就增加前缀和并在后缀和减去相应的数。

## 代码

```cpp
class Solution {
public:
    int pivotInteger(int n) {
        int a = 0, b = (1 + n) * n / 2;
        for(int i = 1; i <= n; i++){
            a += i;
            if(a == b) return i;
            b -= i;
        }
        return -1;
    }
};
```

# **[6246. 追加字符以获得子序列](https://leetcode.cn/problems/append-characters-to-string-to-make-subsequence/)(Easy)**

## 题面

给你两个仅由小写英文字母组成的字符串 `s` 和 `t` 。

现在需要通过向 `s` 末尾追加字符的方式使 `t` 变成 `s` 的一个 **子序列** ，返回需要追加的最少字符数。

子序列是一个可以由其他字符串删除部分（或不删除）字符但不改变剩下字符顺序得到的字符串。

## Sample

### Input:

```cpp
s = "coaching", t = "coding"
```

### Output:

```cpp
4
```

### 范围

- `1 <= s.length, t.length <= 105`
- `s` 和 `t` 仅由小写英文字母组成

## 思路

这道题当时用的双指针的做法，为什么用双指针呢？因为我们要求子序列，那么我们就先求出当前t中在s的最长子序列，用双指针的做法，我们就能较快地求出这个长度。

## 代码

```cpp
class Solution {
public:
    int appendCharacters(string s, string t) {
        int i = 0, j = 0;
        int len1 = s.size(), len2 = t.size();
        while(i < len1 && j < len2){
            if(s[i] == t[j]){
                i++;
                j++;
            }else{
                i++;
            }
        }
        return len2 - j;//j代表t在s中的连续最长子序列
    }
};
```

# **[6247. 从链表中移除节点](https://leetcode.cn/problems/remove-nodes-from-linked-list/)(Medium)**

## 题面

给你一个链表的头节点 `head` 。

对于列表中的每个节点 `node` ，如果其右侧存在一个具有 **严格更大** 值的节点，则移除 `node` 。

返回修改后链表的头节点 **`head` **。

## Sample

### Input:

```cpp
head = [5,2,13,3,8]
```

### Output:

```cpp
[13,8]
```

- 给定列表中的节点数目在范围 `[1, 105]` 内
- `1 <= Node.val <= 105`

## 思路

在打周赛的过程中，看见这道题，第一反应，链表，那么我们就可以将这个链表存储到数组中进行操作。因为函数只传入链表头，不管我们对他的操作。

然后题目要求我们删除`node`，如果这个`node`的右侧有比这个`node`所存储的值更大的，那么我们还一种思路，就是从后往前遍历，找到当前已遍历过的数字中的最大值，依次把它放进数组。然后根据数组里的数字开始创造目标链表。

## 做法

首先我们将链表的数依次存储到数组`nums`中，然后再根据题目的规定从后往前遍历找到对应的数字，最后再将数组里的数字组合成链表。

## 代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNodes(ListNode* head) {
        vector<int> nums;
        while(head){
            nums.emplace_back(head->val);
            head = head->next;
        }
        
        vector<int> arrs;
        int flag = 0;
        for(int i = nums.size() - 1; i >= 0; i--){
            if(nums[i] >= flag) {
                arrs.emplace_back(nums[i]);
                flag = nums[i];
            }
        }
        reverse(arrs.begin(), arrs.end());
        ListNode* root = new ListNode(arrs[0]);
        ListNode* tmp, *p = root;
        for(int i = 1; i < arrs.size(); i++){
            tmp = new ListNode(arrs[i]);
            p->next = tmp;
            p = p->next;
        }
        return root;
    }
};
```

# **[6248. 统计中位数为 K 的子数组](https://leetcode.cn/problems/count-subarrays-with-median-k/)(Hard)**

## 题面

给你一个长度为 `n` 的数组 `nums` ，该数组由从 `1` 到 `n` 的 **不同** 整数组成。另给你一个正整数 `k` 。

统计并返回 `num` 中的 **中位数** 等于 `k` 的非空子数组的数目。

**注意：**

- 数组的中位数是按 **递增** 顺序排列后位于 **中间** 的那个元素，如果数组长度为偶数，则中位数是位于中间靠 **左** 的那个元素。
    - 例如，`[2,3,1,4]` 的中位数是 `2` ，`[8,4,3,5,1]` 的中位数是 `4` 。
- 子数组是数组中的一个连续部分。

## Sample

### Input:

```cpp
nums = [3,2,1,4,5], k = 4
```

### Output:

```cpp
3
```

- `n == nums.length`
- `1 <= n <= 1e5`
- `1 <= nums[i], k <= n`
- `nums` 中的整数互不相同

## 思路

看到数据是`1e5`，那么这道题就不能用 $O(n^2)$的时间复杂度做，所以暴力的方法行不通。

那么我们可以考虑到前缀和的思路，记录`k`左边和右边的数组中大于`k`和小于`k`的数分别有多少，因为`nums`中的整数互不相同，那么这个操作的存在是有意义的。在这个限制条件下，这道题的难度就大大降低了。

## 做法

首先我们得找到这个`k`的索引值`idx`，这样我们就能将整个数组分为两部分，一部分是`k`的左边，另一部分是`k`的右边。

然后我们用前缀和的思想，我们在`[idx - 1, 0]`统计每个数据。假设我们遍历到位置`index1`，那么我们只要统计`[index1, idx - 1]`中大于`k`的数字个数`a`和小于`k`的数字个数`b`。根据前缀和的思路，我们统计完前半部分的所有前缀和`a-b`的值的个数。

之后我们遍历`k`的右边，我们每遍历到一个索引`index2`，我们就记录`[idx + 1, index2]`范围内大于`k`的数字个数与小于`k`的数字个数之差x，然后根据`x`的值找到`k`值左边对应的差值的个数。因为我们要保障`k`是`[index1,index2]`的中位数，那么我们只要保证`[index1,index2]`中小于`k`的数字个数为等于大于`k`的数字个数或者小于`k`的数字个数比大于`k`的数字个数小`1`即可。

## 代码

```cpp
class Solution {
public:
    int countSubarrays(vector<int>& nums, int k) {
        int res = 1;
        int idx = -1;
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] == k){
                idx = i;
                break;
            }
        }
        unordered_map<int, int> hash;
        hash[0] = 1;
        int a = 0, b = 0;
        for(int i = idx - 1; i >= 0; i--){
            if(nums[i] > k) a++;
            else b++;
            hash[a - b]++;
        }
        int x = 0, y = 0;
        res += hash[1];
        res += hash[0] - 1;
        // for(auto it : hash){
        //     cout << it.first << ' ' << it.second << endl;
        // }
        for(int i = idx + 1; i < nums.size(); i++){
            if(nums[i] > k) x++;
            else y++;
            // if(a - b == 1) res++;
            res += hash[y - x];
            res += hash[y - x + 1];
        }
        return res;
    }
};
```