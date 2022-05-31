# 0-1BFS

众所周知，在无权图中使用广度优先搜索算法，可以在 $$ O(|E|) $$ 的算法时间复杂度内求解单源最短路径问题，源结点和其他节点间的距离为它们之间的最小边数。我们也可以把这样的图理解为一个有权图，其中每条边的权重为 $$ 1 $$。如果图中不是所有的边都有相同的权重，则不需要一个更通用的算法，比如时间复杂度为 $$ O(|V|^2+|E|) $$ 或 $$ O(|E|log|V|) $$ 的 Dijkstra 算法。

但是，如果权重有更多的约束，我们通常可以做的更好。在本文中，我们将演示如果每条边的权重为 $$ 0 $$ 或 $$ 1 $$，如何使用 BFS 算法在 $$ O(|E|) $$ 时间复杂度下解决单源最短路径问题（SSSP，single-source-shortest path）。

### 算法

通过仔细研究 Dijkstra 算法，并思考该特殊图隐含的结论，我们来开发我们的算法。Dijkstra 的一般形式为（这里的 set 用于作为优先级队列）：

```cpp
d.assign(n, INF);
d[s] = 0;
set<pair<int, int>> q;
q.insert({0, s});
while (!q.empty()) {
    int v = q.begin()->second;
    q.erase(q.begin());

    for (auto edge : adj[v]) {
        int u = edge.first;
        int w = edge.second;

        if (d[v] + w < d[u]) {
            q.erase({d[u], u});
            d[u] = d[v] + w;
            q.insert({d[u], u});
        }
    }
}
```

我们可以注意到源结点 s 与队列中其他两个结点之间的距离之差最多为 $$ 1 $$。特别地，对于每一个 $$u \in Q$$ 有 $$d[v] \le d[u] \le d[v] + 1$$。这是因为，在每次迭代中，我们只将距离相等或距离多一的结点添加到队列中。假设队列中存在一个 $$d[u] - d[v] > 1$$ 的结点 $u$，则 $u$ 必须是通过一个不同的节点 $t$ 插入到队列中，其中 $$d[t] \ge d[u] - 1 > d[v]$$。然而这是不可能的，因为 Dijkstra 算法是以递增顺序迭代结点的。

这意味着，队列中元素的顺序如下所示：
$$
Q = \underbrace{v}_{d[v]}, \dots, \underbrace{u}_{d[v]}, \underbrace{m}_{d[v]+1} \dots \underbrace{n}_{d[v]+1}
$$
这种数据结构如此简单，我们无需使用实际的优先队列，在这里使用一棵平衡二叉树完全是大材小用。我们可以简单地使用一个普通队列，如果边的权重为 $0$，即 $$d[u]=d[v]$$，则在队列头部添加新结点，如果边的权重为 $1$，即 $$d[u] = d[v] + 1$$，则在队列尾部添加新结点。这种方式队列仍将始终保持有序状态。

```cpp
vector<int> d(n, INF);
d[s] = 0;
deque<int> q;
q.push_front(s);
while (!q.empty()) {
    int v = q.front();
    q.pop_front();
    for (auto edge : adj[v]) {
        int u = edge.first;
        int w = edge.second;
        if (d[v] + w < d[u]) {
            d[u] = d[v] + w;
            if (w == 1)
                q.push_back(u);
            else
                q.push_front(u);
        }
    }
}
```

### Dial 算法

如果允许边的权重更大，我们可以进一步扩展它。如果图中边的权重都 $$\le k$$，则队列中结点的距离（从结点 $v$ 到源结点的距离）将最多相差 $k$。因此，我们可以为队列中的结点维护 $k + 1$ 个桶，并且每当对应于最小距离的存储桶变为空时，我们进行循环移位以获得具有下一个更大距离的桶。这个扩展叫做 Dial 算法。

### 练习题目

- [CodeChef - Chef and Reversing](https://www.codechef.com/problems/REVERSE)
- [Labyrinth](https://codeforces.com/contest/1063/problem/B)
- [KATHTHI](http://www.spoj.com/problems/KATHTHI/)
- [DoNotTurn](https://community.topcoder.com/stat?c=problem_statement&pm=10337)
- [Ocean Currents](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=2620)
- [Olya and Energy Drinks](https://codeforces.com/problemset/problem/877/D)
- [Three States](https://codeforces.com/problemset/problem/590/C)
- [Colliding Traffic](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2621)
- [CHamber of Secrets](https://codeforces.com/problemset/problem/173/B)
- [Spiral Maximum](https://codeforces.com/problemset/problem/173/C)
- [Minimum Cost to Make at Least One Valid Path in a Grid](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid)