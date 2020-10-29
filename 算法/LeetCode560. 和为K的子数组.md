> 给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。
>
> 示例 1 :
>
> 输入:nums = [1,1,1], k = 2
> 输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/subarray-sum-equals-k
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int[] preSum = new int[nums.length];
        preSum[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            preSum[i] = preSum[i - 1] + nums[i];
        }
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        int ans = 0;
        for (int i = 0; i < nums.length; i++) {
            Integer cnt = null;
            if ((cnt = map.get(preSum[i] - k)) != null) {
                ans += cnt;
            }
            if ((cnt = map.get(preSum[i])) != null) {
                map.put(preSum[i], cnt + 1);
            } else {
                map.put(preSum[i], 1);
            }
        }
        return ans;
    }
}
```

