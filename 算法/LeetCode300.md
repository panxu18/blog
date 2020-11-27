> 给定一个无序的整数数组，找到其中最长上升子序列的长度。
>
> 示例:
>
> 输入: [10,9,2,5,3,7,101,18]
> 输出: 4 
> 解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/longest-increasing-subsequence

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        int ans = 0;
        for (int i = 0; i < nums.length; i++) {
            int j = Arrays.binarySearch(dp, 0, ans, nums[i]);
            if (j < 0) {
                dp[-j - 1] = nums[i];
                ans = Math.max(ans, -j);
            }
        }
        return ans;
    }
}
```

