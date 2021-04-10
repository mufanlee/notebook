# LCP 31.变换的迷宫

### 题目

某解密游戏中，有一个 `N*M` 的迷宫，迷宫地形会随时间变化而改变，迷宫出口一直位于 `(n-1,m-1)` 位置。迷宫变化规律记录于 `maze` 中，`maze[i]` 表示 `i` 时刻迷宫的地形状态，`"."` 表示可通行空地，`"#"` 表示陷阱。

地形图初始状态记作 `maze[0]`，此时小力位于起点 `(0,0)`。此后每一时刻可选择往上、下、左、右其一方向走一步，或者停留在原地。

小力背包有以下两个魔法卷轴（卷轴使用一次后消失）：

临时消除术：将指定位置在下一个时刻变为空地；
永久消除术：将指定位置永久变为空地。
请判断在迷宫变化结束前（含最后时刻），小力能否在不经过任意陷阱的情况下到达迷宫出口呢？

注意： 输入数据保证起点和终点在所有时刻均为空地。

示例 1：

输入：`maze = [[".#.","#.."],["...",".#."],[".##",".#."],["..#",".#."]]`

输出：`true`

解释：

![maze.gif](https://pic.leetcode-cn.com/1615892239-SCIjyf-maze.gif)


示例 2：

输入：`maze = [[".#.","..."],["...","..."]]`

输出：`false`

解释：由于时间不够，小力无法到达终点逃出迷宫。

示例 3：

输入：`maze = [["...","...","..."],[".##","###","##."],[".##","###","##."],[".##","###","##."],[".##","###","##."],[".##","###","##."],[".##","###","##."]]`

输出：`false`

解释：由于道路不通，小力无法到达终点逃出迷宫。

提示：

- `1 <= maze.length <= 100`
- `1 <= maze[i].length, maze[i][j].length <= 50`
- `maze[i][j] 仅包含 "."、"#"`

### 解题思路

深度优先遍历。

### 代码

```java
class Solution {
    public boolean escapeMaze(List<List<String>> maze) {
        int t = maze.size(), m = maze.get(0).size(), n = maze.get(0).get(0).length();
        boolean[][][][][] vis = new boolean[t][m][n][2][2];
        return dfs(0, 0, maze, 1, 1, 0, -1, -1, vis);
    }

    private boolean dfs(int x, int y, List<List<String>> maze, int p, int q, int t, int qx, int qy, boolean[][][][][] vis) {
        int m = maze.get(0).size(), n = maze.get(0).get(0).length();
        if (x == m - 1 && y == n - 1) {
            return true;
        }

        if (t == maze.size() - 1) {
            return false;
        }

        if (vis[t][x][y][p][q]) return false;
        vis[t][x][y][p][q] = true;
        int[] dx = {0, 0, 1, -1, 0};
        int[] dy = {1, -1, 0, 0, 0};
        for (int i = 0; i < 5; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            if (nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
            if (maze.get(t + 1).get(nx).charAt(ny) == '.') {
                if (dfs(nx, ny, maze, p, q, t + 1, qx, qy, vis)) return true;
                continue;
            }

            if (q == 0 && nx == qx && ny == qy) {
                if (dfs(nx, ny, maze, p, 0, t + 1, qx, qy, vis)) return true;
            } else {
                if (p == 1 && dfs(nx, ny, maze, 0, q, t + 1, qx, qy, vis)) {
                    return true;
                }
                if (q == 1 && dfs(nx, ny, maze, p, 0, t + 1, nx, ny, vis)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

