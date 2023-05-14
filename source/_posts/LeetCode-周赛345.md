---
title: LeetCode 周赛345
categories:
  - LeetCode
tags:
  - 周赛
  - 并查集
  - 记忆化搜索
  - 构造
abbrlink: bcc7d896
date: 2023-05-14 12:56:52
---

# [6430.找出转圈游戏输家](https://leetcode.cn/contest/weekly-contest-345/problems/find-the-losers-of-the-circular-game/)

## 题面

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514112241.png)

## Sample

### Input
```
n = 5, k = 2
```

### Output
```
[4,5]
```

### Explain

```
以下为游戏进行情况：
1）第 1 个朋友接球，第 1 个朋友将球传给距离他顺时针方向 2 步的玩家 —— 第 3 个朋友。
2）第 3 个朋友将球传给距离他顺时针方向 4 步的玩家 —— 第 2 个朋友。
3）第 2 个朋友将球传给距离他顺时针方向 6 步的玩家 —— 第 3 个朋友。
4）第 3 个朋友接到两次球，游戏结束。
```

## 做法

我们用哈希表来存储每个朋友是否接到过球，因为第二次接球的时候游戏就结束了，因此我们不用记录每个朋友接过多少次球，只要记录是否接过球即可。然后每传一次球，就将相应的位置的哈希值置为`1`，直到该位置本身就是`1`时退出传球。

## 代码

```cpp
class Solution {
public:
    vector<int> circularGameLosers(int n, int k) {
        vector<int> ans;
        vector<int> t(n, 0);
        t[0] = 1;
        int f = 0;
        for(int i = 1; ; i++){
            if(t[(i * k + f) % n]) break;
            else {
                t[(i * k + f) % n] = 1;
                f = (i * k + f) % n;
            }
        }
        for(int i = 0; i < n; i++) {
            if(!t[i]){
                ans.emplace_back(i + 1);
            }
        }
        return ans;
    }
};
```

# [6431. 相邻值的按位异或](https://leetcode.cn/contest/weekly-contest-345/problems/neighboring-bitwise-xor/)

## 题面

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514120633.png)

## Sample

### Input
```
derived = [1,1,0]
```

### Output

```
true
```

### Explain

```
能够派生得到 [1,1,0] 的有效原始二进制数组是 [0,1,0] ：
derived[0] = original[0] ⊕ original[1] = 0 ⊕ 1 = 1 
derived[1] = original[1] ⊕ original[2] = 1 ⊕ 0 = 1
derived[2] = original[2] ⊕ original[0] = 0 ⊕ 0 = 0
```


### 范围

- `n == derived.length`
- `1 <= n <= 1e5`
- `derived` 中的值不是 0 就是 1 。

## 思路

刚开始看到这道题的时候愣了一下，因为不知道如何下手，但是一看到这个题是一个二进制，我就当作模拟来做了
开始做题目之前，我们先熟悉一下什么是异或，下面我会提供一个异或表
| a | b | a⊕b |
| -- | -- | -- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

由此我们看到如果我们知道了数值`a`的值和异或的值，那么`b`的值就是唯一的，根据这个特性我们就可以解决这个问题

我们先假设original数组中的第一个数是`0`，然后我们根据提供的derived数组慢慢往后推original每一个位置的值(因为我们已经知道了异或的值和一个异或的参数)，直到最后一个数，然后根据最后一个数推第一个数，因为题目中有说明，derived的最后一个数是original数组中的最后一个数和第一个数异或得到，因此我们推到最后可以获得original的第一个数，如果和我们假设的数一样，那么说明构造成功，否则就重新假设第一个数是`1`再构造一遍。


## 代码
```cpp
class Solution {
public:
    bool doesValidArrayExist(vector<int>& derived) {
        int f = 0, len = derived.size();
        for(int i = 0; i < len; i++){
            if(derived[i] == 0){
                continue;
            }else{
                if(f == 1) f = 0;
                else f = 1;
            }
        }
        if(f == 0) return true;
        f = 1;
        for(int i = 0; i < len; i++){
            if(derived[i] == 0){
                continue;
            }else{
                if(f == 1) f = 0;
                else f = 1;
            }
        }
        if(f == 1) return true;
        return false;
    }
};
```

# [6433. 矩阵中移动的最大次数](https://leetcode.cn/contest/weekly-contest-345/problems/maximum-number-of-moves-in-a-grid/)

## 题意

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514122530.png)


## Sample

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514122603.png)


### Input
```
grid = [[2,4,3,5],[5,4,9,3],[3,4,2,11],[10,9,13,15]]
```

### Output
```
3
```

### Explain
```
可以从单元格 (0, 0) 开始并且按下面的路径移动：
- (0, 0) -> (0, 1).
- (0, 1) -> (1, 2).
- (1, 2) -> (2, 3).
可以证明这是能够移动的最大次数。
```

### 提示

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 1e5`
- `1 <= grid[i][j] <= 1e6`


## 思路

做题第一要点，得看清题意，做完之后和群内大佬聊天，发现很多人都是因为题意没有理清就做，然后贡献了一发wa
题目中已经很明确的说了，是在**第一列的任一位置**，而不是在任意位置，因此起点只能在第一列
然后根据题目的意思，我们可以知道移动的三个方向如下图所示

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514122455.png)

知道这些后我们应该怎么做呢？如果我们单纯的暴力bfs做的话，搜索树节点将会非常多，会导致我们超时（第一次提交的时候因为这个导致TLE），那我们应该怎么节省时间呢？

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514123358.png)

我想到了之前学dp的时候有一个记忆化搜索，这道题也可以使用，因为我们遍历是从左往右一列一列的遍历，所以不同的点遍历到相同的点时所需的步数是一样的，就像上图的`2->4`和`3->4`，都只需要一步，如果`2->4`已经遍历下去，那么`3->4`之后还需要遍历吗？不需要，因为就算遍历下去，这条路径的最优解也就是之前已经遍历过的最优解，属于是重复遍历，因此我们可以对每一个节点进行标记，如果一个点之前遍历过了就可以直接停止往下遍历。
这样的话，因为每一个点最多遍历一次，因此时间复杂度最高也就是$O(m*n)$。

## 做法

我们用dfs的方法进行搜索获取全局最优解。

首先用一个二维数组对grid图进行标记，0为未曾到达过，1为已经遍历过，如果我们到达一个点时，这个点的标记是1，那么说明我们可以停止在这条路径上继续搜索。
每到达一个点的时候，我们根据路径的三个方向继续往下搜索，直到不能继续搜索。
搜索要注意的有以下两点

1. 搜索的起点是第一列
2. 搜索的路径中数字是逐渐变大的

## 代码

```cpp
class Solution {
public:
    vector<vector<int>> f;
    int ans;
    int rows, cols;
    int dx[3] = {-1, 0, 1};
    
    void dfs(vector<vector<int>> &grid, int row, int col){
        if(f[row][col]) return;
        f[row][col] = 1;
        ans = max(ans, col);
        for(int i = 0; i < 3; i++){
            if(dx[i] + row >= 0 && dx[i] + row < rows && col + 1 < cols && grid[dx[i] + row][col + 1] > grid[row][col]){
                dfs(grid, row + dx[i], col + 1);
            }
        }
    }
    
    int maxMoves(vector<vector<int>>& grid) {
        rows = grid.size(), cols = grid[0].size();
        ans = 0;
        f.assign(grid.begin(), grid.end());
        for(int i = 0; i < rows; i++){
            for(int j = 0; j < cols; j++) f[i][j] = 0;
        }
        for(int i = 0; i < rows; i++){
            dfs(grid, i, 0);
        }
        return ans;
    }
};
```

# [6432. 统计完全连通分量的数量](https://leetcode.cn/contest/weekly-contest-345/problems/count-the-number-of-complete-components/)

## 题意

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514124139.png)

## Sample

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514124156.png)

### Input

```
n = 6, edges = [[0,1],[0,2],[1,2],[3,4]]
```

### Output

```
3
```

### Explain
```
如上图所示，可以看到此图所有分量都是完全连通分量。
```

### 提示

- `1 <= n <= 50`
- `0 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
不存在重复的边

## 思路

在做题目之前我们得搞清楚两个概念

什么是连通分量
> 如果在子图中任意两个顶点之间都存在路径，并且子图中没有任何一个顶点与子图外部的顶点共享边，则称其为 连通分量

什么是完全连通分量
> 如果连通分量中每对节点之间都存在一条边，则称其为 完全连通分量

知道上述的要求之后，我们就可以开始球了，如果是求连通分量应该怎么求？很明显，使用并查集，我们利用并查集就可以知道哪些点是相邻的。
那如果要求完全连通分量呢？我们已经知道了并查集可以求连通分量，那么我们只要在此基础上遍历同一个集合中的所有点的邻居数量即可，如果每一个点的邻居数量刚好是集合大小-1，那么就说明这个集合中的点集是完全连通分量

## 做法

我们首先遍历edges数组，遍历的时候不断合并边的两端，合并的时候也要合并两个端点所在集合的集合size
通过并查集，我们已经能获取所有点集和每个点集的size大小了，因此我们再遍历一次所有点，如果这个点的邻居数量不是点集大小-1，那么我们就将这个点集标记为`-1`，说明这个点集不可能是完全连通分量。

## 代码

```cpp
//并查集模板
int p[N]; //存储每个点的祖宗节点

// 返回x的祖宗节点
int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

// 初始化，假定节点编号是1~n
for (int i = 1; i <= n; i ++ ) p[i] = i;

// 合并a和b所在的两个集合：
p[find(a)] = find(b);
```

```cpp
//维护size的并查集
int p[N]; //存储每个点的祖宗节点

// 返回x的祖宗节点
int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

// 初始化，假定节点编号是1~n
for (int i = 1; i <= n; i ++ ) p[i] = i;

// 合并a和b所在的两个集合：
p[find(a)] = find(b);
```


```cpp
class Solution {
public:
    vector<int> p, size;
    
    int find(int x){
        if(p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
    
    int countCompleteComponents(int n, vector<vector<int>>& edges) {
        int len = edges.size();
        vector<int> sum(n);
        p.resize(n);
        size.resize(n);
        
        for(int i = 0; i < n; i++) {
            p[i] = i;
            size[i] = 1;
        }
        
        for(int i = 0; i < len; i++){
            sum[edges[i][0]]++;
            sum[edges[i][1]]++;
            int a = find(edges[i][0]);
            int b = find(edges[i][1]);
            if(a != b) {
                size[b] += size[a];
                p[a] = b;
            }
        }
        
        vector<int> f(n, 0);
        for(int i = 0; i < n; i++){
            if(p[i] != i) f[i] = -1;
            if(sum[i] != size[p[i]] - 1) f[p[i]] = -1;
        }
        int ans = 0;
        for(int i = 0; i < n; i++){
            if(f[i] != -1) ans++;
        }
        
        return ans;
        
    }
};
```
