---
title: LeetCode 1376.通知所有员工所需的时间
categories:
  - LeetCode
tags:
  - Daily
  - Bfs
abbrlink: 3d809fe9
date: 2023-05-01 17:27:33
---

# LeetCode 1376.通知所有员工所需的时间

## 题面

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230501171724.png)

## Sample

### Input

```bash
n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
```

### Output

```bash
1
```

### Explain

```bash
id = 2 的员工是公司的总负责人，也是其他所有员工的直属负责人，他需要 1 分钟来通知所有员工。
上图显示了公司员工的树结构。
```

### 数据范围

- `1 <= n <= 10^5`
- `0 <= headID < n`
- `manager.length == n`
- `0 <= manager[i] < n`
- `manager[headID] == -1`
- `informTime.length == n`
- `0 <= informTime[i] <= 1000`
- 如果员工 `i` 没有下属，`informTime[i] == 0` 。
- 题目 **保证** 所有员工都可以收到通知。

## 思路

这道题我们可以看作是一棵多叉树，或者是一个带边权的单向图，我们要求的是找到这个单向图中最远的两个节点的长度。

我们已经知道了起点，我们要找最远的路径，那么我们只要找到这个点到所有点的距离即可，然后选取最大值，因为图中的点最多是1e5，并且每个点都只有一个入度（除了起点），因此不用考虑重复到达某个点的问题。

## 做法

既然是需要遍历所有点的距离，那么我们可以用bfs和dfs遍历获取起点到每一个点的距离。

这里我选择使用bfs

值得注意的是，我们在bfs的过程中我们需要维护起点到每个点的时间消耗。

## 代码

```cpp
class Solution {
public:
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        vector<vector<int>> employ(n);
        vector<int> costT(n, 0);//起点到其他点消耗的时间
        int len = manager.size();
        for(int i = 0; i < len; i++){
            if(headID == i) continue;
            employ[manager[i]].emplace_back(i);
        }
        int ans = 0;
        queue<int> q;
        q.push(headID);
        while(!q.empty()){
            int v = q.front();
            q.pop();
            for(auto u : employ[v]){
                q.push(u);
                costT[u] = costT[v] + informTime[v];//时间=起点到上一个消耗的时间+上一个点到这个点消耗的时间
            }
        }
        for(int i = 0; i < n; i++){
            ans = max(ans, costT[i]);
        }
        return ans;
    }
};
```