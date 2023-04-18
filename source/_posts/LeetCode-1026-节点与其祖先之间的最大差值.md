---
title: LeetCode 1026.节点与其祖先之间的最大差值
categories:
  - LeetCode
tags:
  - Daily
  - 递归
abbrlink: 98792e2b
date: 2023-04-18 22:28:36
---

# **[1026.节点与其祖先之间的最大差值](https://leetcode.cn/problems/maximum-difference-between-node-and-ancestor/)**

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230418221658.png)

## Sample

### Input

```cpp
root = [8,3,10,1,6,null,14,null,null,4,7,13]
```

### Output

```cpp
7
```

### Explain

```cpp
我们有大量的节点与其祖先的差值，其中一些如下：
|8 - 3| = 5
|3 - 7| = 4
|8 - 1| = 7
|10 - 13| = 3
在所有可能的差值中，最大值 7 由 |8 - 1| = 7 得出
```

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230418221818.png)

### 数据范围

- 树中的节点数在 `2` 到 `5000` 之间。
- `0 <= Node.val <= 1e5`

## 思路

通常树上操作我们都需要进行递归，那么我们想想这道题能不能用递归做。

因为我们要搜索所有的节点与它的祖先之间的差值，那么我们需要用dfs来遍历所有的节点，因此递归是可以做的。

那么问题来了，既然递归可以，那么我们怎么做呢？因为现在的问题是我们如果用平常的递归的话，每次计算差值我们难道需要返回去获得每一个祖先节点的值吗？

其实不用，我们可以用两个变量当作参数传入函数，这两个变量分别存储遍历到这个节点时所有的节点的val值的最大值和最小值，因此我们计算每个节点与祖先节点的差值的时候只要与这两个值进行做差即可。

## 代码

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, int maxx, int minn, int *ans){
        if(root == nullptr) return;
        *ans = max(*ans, max(abs(root->val - maxx), abs(root->val - minn)));
        maxx = max(maxx, root->val);
        minn = min(minn, root->val);
        dfs(root->left, maxx, minn, ans);
        dfs(root->right, maxx, minn, ans);
    }

    int maxAncestorDiff(TreeNode* root) {
        if(root == nullptr) return 0;
        int ans = 0;
        int maxx = root->val, minn = root->val;
        dfs(root->left, maxx, minn, &ans);
        dfs(root->right, maxx, minn, &ans);
        return ans;
    }
};
```