# LCP 28.采购方案

### 题目

小力将 N 个零件的报价存于数组 `nums`。小力预算为 `target`，假定小力仅购买两个零件，要求购买零件的花费不超过预算，请问他有多少种采购方案。

注意：答案需要以 `1e9 + 7 (1000000007)` 为底取模，如：计算初始结果为：`1000000008`，请返回 `1`

示例 1：

输入：`nums = [2,5,3,5], target = 6`

输出：`1`

解释：预算内仅能购买 nums[0] 与 nums[2]。

示例 2：

输入：`nums = [2,2,1,9], target = 10`

输出：`4`

解释：符合预算的采购方案如下：
nums[0] + nums[1] = 4
nums[0] + nums[2] = 3
nums[1] + nums[2] = 3
nums[2] + nums[3] = 10

提示：

- `2 <= nums.length <= 10^5`
- `1 <= nums[i], target <= 10^5`

### 解题思路

为了求得购买的两个零件不超过预算的采购方案数，我们需要求出数组 `nums` 中的每个元素 `i`，使其满足`nums[i] + nums[j] <= target` 的 `j` 的数量。计算数组中满足 `nums[i] + nums[j] <= target` 的 `j` 的数量很像题目 `两数之和`。我们可以使用二分查找或者双指针的方法。

#### 方法一：二分查找

- 对数组 `nums` 进行排序；
- 遍历 `nums`, 对于第 `i` 个元素：
    - 二分查找 `i` 之前的元素中与其之和小于等于 `target` 的元素数目，即二分查找 `[0, i)` 中 `target - nums[i]` 的上界；
    - 累加第 `i` 个元素满足预算的方案数；
- 返回`总方案数 % 1000000007`。

#### 方法二：双指针

- 对数组 `nums` 进行排序；
- 初始化双指针 `l = 0, r = nums.length - 1`；
- 当 `l < r` 时：
    - 如果 `nums[i] + nums[j] > target`，则 `r--`；
    - 否则，`(i,j]` 范围内的元素与 `l` 处元素之和皆满足要求，即有`j - i`个方案，并将`l++`； 
- 返回总方案数。

### 代码

```java
class Solution {
    public int purchasePlans(int[] nums, int target) {
        final int MOD = 1000000007;
        Arrays.sort(nums);
        long ans = 0;
        for (int i = 0; i < nums.length; i++) {
            int index = upperBound(nums, target - nums[i], 0, i) + 1;
            ans = ans + index;
        }
        return (int) (ans % MOD);
    }

    public int upperBound(int[] nums, int target, int left, int right) {
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left - 1;
    }
}
```

```java
class Solution {
    public int purchasePlans(int[] nums, int target) {
        final int MOD = 1000000007;
        Arrays.sort(nums);
        int ans = 0, l = 0, r = nums.length - 1;
        while (l < r) {
            if (nums[l] + nums[r] > target) {
                r--;
            } else {
                ans += r - l;
                ans %= MOD;
                l++;
            }
        }
        return ans;
    }
}
```

