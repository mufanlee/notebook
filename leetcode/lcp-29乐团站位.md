# LCP 29.乐团站位

### 题目

某乐团的演出场地可视作 `num * num` 的二维矩阵 `grid`（左上角坐标为 `[0,0]`)，每个位置站有一位成员。乐团共有 `9` 种乐器，乐器编号为 `1~9`，每位成员持有 `1` 个乐器。

为保证声乐混合效果，成员站位规则为：自 `grid` 左上角开始顺时针螺旋形向内循环以 `1，2，...，9` 循环重复排列。例如当 `num = 5` 时，站位如图所示

![image.png](https://pic.leetcode-cn.com/1616125411-WOblWH-image.png)

请返回位于场地坐标 `[Xpos,Ypos]` 的成员所持乐器编号。

示例 1：

输入：`num = 3, Xpos = 0, Ypos = 2`

输出：`3`

解释：

![image.png](https://pic.leetcode-cn.com/1616125437-WUOwsu-image.png)


示例 2：

输入：`num = 4, Xpos = 1, Ypos = 2`

输出：`5`

解释：

![image.png](https://pic.leetcode-cn.com/1616125453-IIDpxg-image.png)


提示：

- `1 <= num <= 10^9`
- `0 <= Xpos, Ypos < num`

### 解题思路

数学题，找规律，求得从`[0,0]` 到 `[Xpos,Ypos]` 总的位置数量，然后对 `9` 取余。

### 代码

```java
class Solution {
    public int orchestraLayout(int num, int xPos, int yPos) {
        int a = Math.min(xPos + 1, num - xPos);
        int b = Math.min(yPos + 1, num - yPos);
        long k = Math.min(a, b);
//        System.out.println(k);
        long sum = 4L * ((k - 1) * num - (k - 1) * (k - 1));
//        System.out.println(sum);
        long x = k - 1, y = k - 1;
        long m = num - 2 * (k - 1);
        if (m == 1) {
            return answer(sum + 1);
        }
        if (isIn(xPos, yPos, k - 1, k - 1, k - 1, num - k - 1)) {
            sum += yPos - (k - 1) + 1;
            return answer(sum);
        } else {
            sum += m - 1;
        }

        if (isIn(xPos, yPos, k - 1, num - k, num - k - 1, num - k)) {
            sum += xPos - (k - 1) + 1;
            return answer(sum);
        } else {
            sum += m - 1;
        }

        if (isIn(xPos, yPos, num - k, k - 1 + 1, num - k, num - k)) {
            sum += (num - k - yPos) + 1;
            return answer(sum);
        } else {
            sum += m - 1;
        }

        if (isIn(xPos, yPos, k - 1 + 1, k - 1, num - k, k - 1)) {
            sum += (num - k - xPos) + 1;
            return answer(sum);
        } else {
            sum += m - 1;
        }
        return answer(sum);
    }

    private int answer(long sum) {
//        System.out.println(sum);
        return sum % 9 == 0 ? 9 : (int) (sum % 9);
    }

    private boolean isIn(long x, long y, long x1, long y1, long x2, long y2) {
        return x >= x1 && x <= x2 && y >= y1 && y <= y2;
    }
}
```

