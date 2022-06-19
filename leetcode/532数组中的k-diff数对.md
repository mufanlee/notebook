# [532. 数组中的 k-diff 数对](https://leetcode.cn/problems/k-diff-pairs-in-an-array/)

### 题目（中等）

给你一个整数数组 `nums` 和一个整数 `k`，请你在数组中找出 **不同的** k-diff 数对，并返回不同的 **k-diff 数对** 的数目。

**k-diff** 数对定义为一个整数对 `(nums[i], nums[j])` ，并满足下述全部条件：

- `0 <= i, j < nums.length`
- `i != j`
- `nums[i] - nums[j] == k`

**注意**，`|val|` 表示 `val` 的绝对值。

 

示例 1：

```
输入：nums = [3, 1, 4, 1, 5], k = 2
输出：2
解释：数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
尽管数组中有两个 1 ，但我们只应返回不同的数对的数量。
```


示例 2：

```
输入：nums = [1, 2, 3, 4, 5], k = 1
输出：4
解释：数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5) 。
```


示例 3：

```
输入：nums = [1, 3, 1, 5, 4], k = 0
输出：1
解释：数组中只有一个 0-diff 数对，(1, 1) 。
```



提示：

- `1 <= nums.length <= 10^4`
- `-10^7 <= nums[i] <= 10^7`
- `0 <= k <= 10^7`

### 解题思路

#### 方法一：哈希表

- 维护两个哈希表，$vis$ 存储遍历过的数组中的元素，$ans$ 存储符合要求的k-diff 数对；
- 遍历数组中的元素，对于当前元素 $x$，查找 $vis$ 中是否包含 $x - k$ 和 $x + k$，包含时，数对以其中最小的值作为唯一标识存到 $ans$ 中；
- 最后返回 $ans$ 的大小。

##### 复杂度分析

- 时间复杂度：$O(n)$。
- 空间复杂度：$O(n)$。

#### 方法二：排序+双指针

k-diff 数对不要求两个元素有序，只要两个元素下标值不同，即数组中处于不同位置即可。因此，我们可以先对数组进行排序，再利用双指针查找每个元素可以构成 k-diff数对的元素。

- 对数组 $nums$ 排序；
- 定义指针 $i$ 指向数组中的元素，指针 $j$ 用于查找 $i$ 位置之后可以与其构成 k-diff 对的元素位置；
- 使用双指针遍历数组 $nums$，寻找 k-diff 对。
- 遍历过程中注意跳过重复的值。
- 注意：由于数组中的元素是有序的，设与 $i$ 处元素构成 k-diff 的元素位于 $j$，则与 $i + 1$ 处元素构成 k-diff 的元素位置一定大于等于 $j$，因此指针 $j$ 不必每次从 $i + 1$ 开始遍历，从上次找到 k-diff 的位置开始遍历即可。

##### 复杂度分析

- 时间复杂度：$O(nlogn)$。
- 空间复杂度：$O(1)$。

### 代码

```java
class Solution {
    public int findPairs(int[] nums, int k) {
        Set<Integer> vis = new HashSet<>();
        Set<Integer> ans = new HashSet<>();
        for (int x : nums) {
            if (vis.contains(x - k)) {
                ans.add(x - k);
            }
            if (vis.contains(x + k)) {
                ans.add(x);
            }
            vis.add(x);
        }
        return ans.size();
    }
}
```

```java
class Solution {
    public int findPairs(int[] nums, int k) {
        int n = nums.length;
        Arrays.sort(nums);
        int ans = 0;
        for (int i = 0, j = 1; i < n && j < n; i++) {
            // 跳过重复值
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            // 有可能去重时导致 i >= j
            if (j <= i) j = i + 1;
            // 查找符合要求的 j
            while (j < n && nums[j] - nums[i] < k) j++;
            if (j < n && nums[j] - nums[i] == k) {
                ans++;
            }
        }
        return ans;
    }
}
```

