# Dijkstra-寻找最短路

给定一个有向或无向加权图，其中包含 $n$ 个顶点和 $m$ 条边。所有边的权重均为非负数。另外，给一个起始顶点 $s$。本文讨论查找从起始顶点 $s$ 到所有其他顶点的最短路径的长度，并输出最短路径。

此问题也称为单源最短路径问题。

### 算法

如下是荷兰计算机科学家Edsger W. Dijkstra在1959年描述的算法。

创建一个数组 $d[]$，对于图中的每个顶点 $v$，我们将从源点 $s$ 到 $v$ 的最短路径的当前长度存储在 $d[v]$ 中。初始时，$d[s] = 0$，对于其他顶点，该长度为无穷大。在实现时，我们可以选择一个足够大的数（保证大于任何可能的路径长度）作为无穷大。

$$d[v] = \infty, ~ v \ne s$$

此外，我们维护一个布尔数组 $u[]$，该数组存储每个顶点 $v$ 是否标记。最初所有顶点都未被标记：

$$u[v] = {\rm false}$$

Dijkstra 算法运行 $n$ 次迭代。在每次迭代中，一个顶点 $v$ 被选为具有最小值 $d[v]$ 的未标记结点：

显然，在第一次迭代中，将选择起始顶点 $s$。

所选顶点 $v$ 被标记。接下来，从顶点 $v$ 执行 **松弛** ：考虑形式为 $(v,\text{to})$ 的所有边，对于每个顶点 $\text{to}$ 算法会尝试改进值 $d[\text{to}]$。假设当前边长度为 $len$，则松弛的代码为：

$$d[\text{to}] = \min (d[\text{to}], d[v] + len)$$

在考虑所有这样的边后，当前迭代结束。最后，在 $n$ 次迭代后，所有顶点将被标记，算法终止。此时，我们找到了从 $s$ 到所有顶点 $v$ 的最短路径 $d[v]$。

请注意，如果某些顶点无法从起始顶点 $s$ 到达，则它们的值 $d[v]$ 将保持无穷大。显然，算法的最后几次迭代将选择这些顶点，但不会为它们做任何有用的工作。因此，只要所选顶点与它有无穷大的距离，就可以停止算法。

### 恢复最短路径

通常，人们不仅需要知道最短路径的长度，还需要知道最短路径本身。让我们看看如何维护足够的信息，以恢复从 $s$ 到任何顶点的最短路径。我们维护一个前序顶点数组 $p[]$，其中对于每个顶点 $v \ne s$，$p[v]$ 是从 $s$ 到 $v$ 的最短路径上的倒数第二个顶点。即如果我们采用某个顶点 $v$ 的最短路径并从此路径上删除 $v$，我们将得到一个以顶点 $p[v]$ 结尾的路径，并且此路径将是顶点 $p[v]$ 的最短路径。此前序顶点数组可用于将最短路径恢复到任何顶点：从 $v$ 开始，重复获取当前顶点的前序顶点，直到到达起始顶点 $s$ ，$s$ 到 $v$ 的最短路径即以相反顺序列出获取的这些顶点。因此，到顶点 $v$ 的最短路径 $P$ 等于：

$$P = (s, \ldots, p[p[p[v]]], p[p[v]], p[v], v)$$

构建这个前序顶点数组非常简单：对于每个成功的松弛操作，即当对某些选定的顶点 $v$ 时，到某个顶点 $\text{to}$ 的有所改善时，我们将  $\text{to}$ 的前序顶点更新为顶点 $v$：

$$p[\text{to}] = v$$

### 证明

Dijkstra 算法正确性所基于的主要断言如下：

**标记任何顶点 $v$ 后，到该顶点 $d[v]$ 的当前距离是最短的，并且将不再更改。**

可以通过归纳法证明。对于第一次迭代，这个断言是显而易见的：唯一标记的顶点是 $s$，距离 $d[s]=0$ 确实是到 $s$ 的最短路径长度。现在假设这个断言对所有先前的迭代是正确的，即对于所有已标记的顶点；让我们证明在当前迭代完成后它没有被违反。设 $v$ 是当前迭代中选择的顶点，即 $v$ 是算法将标记的顶点。现在我们必须证明 $d[v]$ 确实等于到它 $l[v]$ 的最短路径长度。

考虑顶点 $v$ 的最短路径 $P$，这个路径可以分成两部分：$P_1$ ，它仅由标记的顶点组成（至少起始顶点 $s$ 是 $P_1$ 的一部分），以及路径的其余部分 $P_2$ （它可能包括一个标记的顶点，但它总是以未标记顶点开头）。让我们将路径 $P_2$ 的第一个顶点表示为 $p$，将路径 $P_1$ 的最后一个顶点表示为 $q$。

首先，我们证明我们的断言对顶点 $p$ 是有效的，即证明 $d[p] = l[p]$。这几乎是显而易见的：在之前一次迭代中，我们选择了顶点 $q$ 并从它执行了松弛操作。由于（通过选择顶点 $p$）到 $p$ 的最短路径是到 $q$ 的最短路径加上边 $(p,q)$，从 $q$ 的松弛操作设置 $d[p]$ 为最短路径长度 $l[p]$。

由于边的权重是非负的，因此最短路径 $l[p]$ （我们刚刚证明等于 $d[p]$）不超过到顶点 $v$ 的最短路径长度 $l[v]$。给定 $l[v] \le d[v]$ （因为 Dijkstra 的算法不可能找到比最短的可能方法更短的方法），我们得到不等式：

$$d[p] = l[p] \le l[v] \le d[v]$$

另一方面，由于顶点 $p$ 和 $v$ 都未标记，并且当前迭代选择顶点 $v$ 而不是 $p$，因此我们得到另一个不等式：

$$d[p] \ge d[v]$$

从这两个不等式中，我们得出结论 $d[p] = d[v]$，然后从先前发现的方程中我们得到：

$$d[v] = l[v]$$

Q.E.D.

### 实现

Dijkstra 算法执行 $n$ 次迭代。在每次迭代中，它选择一个具有最小值的未标记顶点 $v$，对其进行标记并检查所有边 $(v, \text{to})$，尝试改进值 $d[\text{to}]$。

该算法的运行时间包括：

- 为一个具有最小值 $d[v]$ 的顶点在 $O(n)$ 未标记顶点中进行 $n$ 次搜索
- $m$ 次松弛尝试

对于这些操作最简单的实现，在每次迭代中顶点搜索需要 $O(n)$ 次操作，每次松弛在 $O(1)$ 执行。该算法的渐进时间复杂度为：

$$O(n^2+m)$$ 

这个复杂度对于稠密图是最佳的，即 $m \approx n^2$。

然而在稀疏图中，当 $m$ 远小于最大边数 $n^2$ 时，这个问题可以在 $O(n \log n + m)$ 复杂度解决。算法实现可以在**稀疏图上的Dijkstra**一文中找到。

```{.cpp file=dijkstra_dense}
const int INF = 1000000000;
vector<vector<pair<int, int>>> adj;

void dijkstra(int s, vector<int> & d, vector<int> & p) {
    int n = adj.size();
    d.assign(n, INF);
    p.assign(n, -1);
    vector<bool> u(n, false);

    d[s] = 0;
    for (int i = 0; i < n; i++) {
        int v = -1;
        for (int j = 0; j < n; j++) {
            if (!u[j] && (v == -1 || d[j] < d[v]))
                v = j;
        }
        
        if (d[v] == INF)
            break;
        
        u[v] = true;
        for (auto edge : adj[v]) {
            int to = edge.first;
            int len = edge.second;
            
            if (d[v] + len < d[to]) {
                d[to] = d[v] + len;
                p[to] = v;
            }
        }
    }
}
```

在这里图 $\text{adj}$ 以邻接链表的形式存储：对于每个顶点 $v$，$\text{adj}$ 包含从此顶点出发的边列表，即 `pair<int,int>` 的列表，其中 `pair` 中的第一个元素是边的另一端，第二个元素是边的权重。

该函数以起始顶点 $s$ 和两个 `vector` 为参数，其中这个两个 `vector` 将用于返回值。

首先，代码初始化数组：距离数组 $d[]$，标记数组 $u[]$ 和前序数组 $p[]$。然后，它只需 $n$ 轮迭代。在每轮迭代中，在所有未标记顶点中具有最短距离 $d[v]$ 的顶点 $v$ 被选择。如果到被选中顶点 $v$ 的距离等于无穷大，则算法停止。否则，将标记顶点，并且检查所有从该顶点出发的边。如果沿边松弛（即距离 $d[\text{to]}]$ 可以改进），则更新距离 $d[\text{to}]$ 和前序顶点 $p[\text{to}]$。

执行所有迭代后，数组 $d[]$ 存储所有顶点的最短路径长度，数组 $p[]$ 存储所有顶点的前序顶点（起始顶点 $s$ 除外）。可以通过以下方式恢复任何顶点 $t$ 的路径：

```{.cpp file=dijkstra_restore_path}
vector<int> restore_path(int s, int t, vector<int> const& p) {
    vector<int> path;

    for (int v = t; v != s; v = p[v])
        path.push_back(v);
    path.push_back(s);

    reverse(path.begin(), path.end());
    return path;
}
```

### 参考

* Edsger Dijkstra. A note on two problems in connexion with graphs [1959]
* Thomas Cormen, Charles Leiserson, Ronald Rivest, Clifford Stein. Introduction to Algorithms [2005]

### 练习题目

* [Timus - Ivan's Car](http://acm.timus.ru/problem.aspx?space=1&num=1930) [Difficulty:Medium]
* [Timus - Sightseeing Trip](http://acm.timus.ru/problem.aspx?space=1&num=1004)
* [SPOJ - SHPATH](http://www.spoj.com/problems/SHPATH/) [Difficulty:Easy]
* [Codeforces - Dijkstra?](http://codeforces.com/problemset/problem/20/C) [Difficulty:Easy]
* [Codeforces - Shortest Path](http://codeforces.com/problemset/problem/59/E)
* [Codeforces - Jzzhu and Cities](http://codeforces.com/problemset/problem/449/B)
* [Codeforces - The Classic Problem](http://codeforces.com/problemset/problem/464/E)
* [Codeforces - President and Roads](http://codeforces.com/problemset/problem/567/E)
* [Codeforces - Complete The Graph](http://codeforces.com/problemset/problem/715/B)
* [TopCoder - SkiResorts](https://community.topcoder.com/stat?c=problem_statement&pm=12468)
* [TopCoder - MaliciousPath](https://community.topcoder.com/stat?c=problem_statement&pm=13596)
* [SPOJ - Ada and Trip](http://www.spoj.com/problems/ADATRIP/)
* [LA - 3850 - Here We Go(relians) Again](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1851)
* [GYM - Destination Unknown (D)](http://codeforces.com/gym/100625)
* [UVA 12950 - Even Obsession](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=4829)
* [GYM - Journey to Grece (A)](http://codeforces.com/gym/100753)
* [UVA 13030 - Brain Fry](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=866&page=show_problem&problem=4918)
* [UVA 1027 - Toll](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=3468)
* [UVA 11377 - Airport Setup](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=2372)
* [Codeforces - Dynamic Shortest Path](http://codeforces.com/problemset/problem/843/D)
* [UVA 11813 - Shopping](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2913)
* [UVA 11833 - Route Change](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=226&page=show_problem&problem=2933)
* [SPOJ - Easy Dijkstra Problem](http://www.spoj.com/problems/EZDIJKST/en/)
* [LA - 2819 - Cave Raider](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=820)
* [UVA 12144 - Almost Shortest Path](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=3296)
* [UVA 12047 - Highest Paid Toll](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3198)
* [UVA 11514 - Batman](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=2509)
* [Codeforces - Team Rocket Rises Again](http://codeforces.com/contest/757/problem/F)
* [UVA - 11338 - Minefield](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2313)
* [UVA 11374 - Airport Express](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2369)
* [UVA 11097 - Poor My Problem](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2038)
* [UVA 13172 - The music teacher](https://uva.onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=5083)
* [Codeforces - Dirty Arkady's Kitchen](http://codeforces.com/contest/827/problem/F)
* [SPOJ - Delivery Route](http://www.spoj.com/problems/DELIVER/)
* [SPOJ - Costly Chess](http://www.spoj.com/problems/CCHESS/)
* [CSES - Shortest Routes 1](https://cses.fi/problemset/task/1671)
* [CSES - Flight Discount](https://cses.fi/problemset/task/1195)
* [CSES - Flight Routes](https://cses.fi/problemset/task/1196)
