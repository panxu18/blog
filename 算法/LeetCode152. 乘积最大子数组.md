> 给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。
>
>  
>
> 示例 1:
>
> 输入: [2,3,-2,4]
> 输出: 6
> 解释: 子数组 [2,3] 有最大乘积 6。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/maximum-product-subarray
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

计算以第i个数nums[i]结尾的最大子数组乘积dp[i],可以分为3中情况：

1. nums[i]
2. 以nums[i-1]结尾的最大子数组正乘积dp‘[i-1] * nums[i]
3. 以nums[i-1]结尾的最大子数组负乘积dp‘‘[i-1] * nums[i]

```java
class Solution {
    public int maxProduct(int[] nums) {
        int dp1 = nums[0];
        int dp2 = nums[0];
        int ans = nums[0];
        for (int i = 1; i < nums.length; i++) {
            int temp1 = dp1, temp2 = dp2, num = nums[i];
            dp1 = Math.max(num, Math.max(temp1 * num, temp2 * num));
            dp2 = Math.min(num, Math.min(temp1 * num, temp2 * num));
            ans = Math.max(ans, dp1);
        }
        return ans;

    }
}
```

