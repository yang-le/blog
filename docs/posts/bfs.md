---
date:
  created: 2026-05-10
categories:
    - 算法
---

# 广度优先搜索算法

广度优先搜索(BFS)是一类借助队列在图上进行搜索的算法。其基本步骤如下：

<!-- more -->

1. 将起点推入队列。注意起点可以有多个，这称为多源BFS.
2. 循环直到队列为空（或满足特定条件）

    1. 队首出队
    2. 队首的（满足特定条件的）邻接点入队

例题：[地图中的最高点](https://leetcode.cn/problems/map-of-highest-peak/description/)

题目大意：在一个网格图中，规定水面的高度为零，且相邻格子的高度差最多为1，求网格图中的最高高度。要求将所有的高度数据返回。参考代码如下：

``` cpp
class Solution {
public:
    vector<vector<int>> highestPeak(vector<vector<int>>& isWater) {
        int m = isWater.size(), n = isWater[0].size();
        queue<pair<int, int>> q;
        vector ans(m, vector<int>(n, -1));  // (1)!
        
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (isWater[i][j]) {
                    q.emplace(i, j); // (2)!
                    ans[i][j] = 0;
                }

        constexpr int dirs[][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

        while (!q.empty()) {
            auto [i, j] = q.front();
            q.pop();

            for (auto [di, dj] : dirs) {
                int ni = i + di, nj = j + dj;
                if (0 <= ni && ni < m && 0 <= nj && nj < n && ans[ni][nj] == -1) {
                    q.emplace(ni, nj);  // (3)!
                    ans[ni][nj] = ans[i][j] + 1; // (4)!
                }
            }
        }
        return ans;
    }
};
```

1. `-1`表示尚未求得高度
2. 水面节点入队，这是多源BFS
3. 如果邻接节点在网格内，且其高度尚未求得，入队
4. 更新邻接节点的高度，为使其最大，我们令相邻格子的高度比当前格子高一个单位

### 复杂度分析
- 时间复杂度：$O(mn)$
- 空间复杂度：$O(mn)$

## 使用双端队列的广度优先搜索

此算法也称0-1BFS. 我们直接看一道例题：
[穿越网格图的安全路径](https://leetcode.cn/problems/find-a-safe-walk-through-a-grid/description/)

题目大意：一个只有0或1两种值的网格，此值代表经过该网格所需消耗的健康值。问使用给定的健康值能否从网格图的左上角走到右下角。参考代码如下：

``` cpp
class Solution {
public:
    bool findSafeWalk(vector<vector<int>>& grid, int health) {
        int m = grid.size(), n = grid[0].size();

        deque<tuple<int, int, int>> q;
        q.emplace_back(grid[0][0], 0, 0);

        vector vis(m, vector<int8_t>(n));
        vis[0][0] = true;

        constexpr int dirs[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

        while (!q.empty()) {
            auto [h, x, y] = q.front();
            q.pop_front();

            if (x == m - 1 && y == n - 1)   // (1)!
                return h < health;

            for (auto [dx, dy] : dirs) {
                int nx = x + dx, ny = y + dy;
                if (0 <= nx && nx < m && 0 <= ny && ny < n && !vis[nx][ny]) {
                    vis[nx][ny] = true;
                    if (grid[nx][ny] == 1)
                        q.emplace_back(h + 1, nx, ny);  // (2)!
                    else
                        q.emplace_front(h, nx, ny);     // (3)!
                }
            }
        }
        return false;
    }
};
```

1. 如果出队的是右下角的格子，直接退出
2. 如果访问的网格需要消耗健康值，插入队尾
3. 如果访问的网格不需要消耗健康值，插入队首

### 复杂度分析
- 时间复杂度：$O(mn)$
- 空间复杂度：$O(mn)$

## 稀疏图的Dijkstra算法

当图的任意两个节点几乎都有边连接时，称为稠密图；反之称为稀疏图。稠密图的边的数量级和$n^2$相当，或者说边数$m$为$O(n^2)$.

Dijkstra是用于求非负边权的单源最短路的一种算法。当图稀疏时，使用优先队列（堆）优化的Dijkstra算法的复杂度为$O(m\log m)$；但当图稠密时，应当使用朴素Dijkstra算法，其复杂度为$O(n^2)$. 

优先队列优化的Dijkstra算法基本步骤如下：

1. 将起点$(d_s, s)$推入优先队列
2. 循环直到队列为空或到达终点

    1. 队首出队
    2. 更新邻接点的距离
    3. 邻接点入队

注意当所有边权相等时，优先队列就退化为普通队列，上述算法就退化为普通的BFS.

例题：[网络延迟时间](https://leetcode.cn/problems/network-delay-time/description/)

题目大意：求有向图中的节点$K$到距其最远的节点的距离。参考代码如下：

``` cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        vector<int> dis(n, INT_MAX);

        pq.emplace(0, k - 1);
        dis[k - 1] = 0;

        vector<vector<pair<int, int>>> g(n);
        for (auto& t : times)
            g[t[0] - 1].emplace_back(t[2], t[1] - 1);

        while (!pq.empty()) {
            auto [d, x] = pq.top();
            pq.pop();
            // (1)!
            if (d > dis[x]) // (2)!
                continue;
            for (auto [dt, nx] : g[x])
                if (d + dt < dis[nx])
                    pq.emplace(dis[nx] = d + dt, nx); // (3)!
        }

        int ans = ranges::max(dis);
        return ans == INT_MAX ? -1 : ans;
    }
};
```

1. 如果终点只有一个，可在这里判断`x`是否就是终点，如果是则提前退出循环。
2. 这里用了懒删除的技巧。即需要删除堆中的节点时，并不是立即删除它们（这需要O(n)的时间），而是等到节点出堆时才进行真正的删除操作（仅需要O(log n)的时间）。使用这个技巧优化的堆也称为**懒删除堆**。
3. 这里用了懒删除的技巧。本来现在堆中记录的距离大于`d + dt`的所有`nx`节点应该被立即删除，因为其真实距离已经被更新，那些节点都无效了。

### 复杂度分析
- 时间复杂度：$O(m\log m)$
- 空间复杂度：$O(m)$

## 总结

就像深度优先搜索算法离不开栈一样，广度优先搜索算法离不开队列。本文分别介绍了使用普通队列，双端队列和优先队列三种风格不同的广度优先搜索。处理实际问题时，要看本质上有几种边权。当边权可以任意取值时，使用优先队列；当边权只有两种时，使用双端队列；当所有边权都相同时，使用普通队列即可。需要注意的是，使用优先队列的BFS虽然适用性更广，但时间复杂度也更高。
