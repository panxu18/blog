> 给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
>
> 注意:
>
> 每个数组中的元素不会超过 100
> 数组的大小不会超过 200
> 示例 1:
>
> ```text
> 输入: [1, 5, 11, 5]
> 
> 输出: true
> 
> 解释: 数组可以分割成 [1, 5, 5] 和 [11].
> ```
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/partition-equal-subset-sum
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int total = Arrays.stream(nums).sum();
        if ((total & 1) == 1) {
            return false;
        }
        int halfSum = total >> 1;
        boolean [] dp = new boolean[halfSum  + 1];
        dp[0] = true;
        for (int i = 0; i < nums.length; i++) {
            for (int j = halfSum; j >= nums[i]; j--) {
                dp[j] |= dp[j - nums[i]];
            }
        }
        return dp[halfSum];
    }
}
```

