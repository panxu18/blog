> 返回 A 的最短的非空连续子数组的长度，该子数组的和至少为 K 。
>
> 如果没有和至少为 K 的非空子数组，返回 -1 。
>
>  
>
> 示例 1：
>
> 输入：A = [1], K = 1
> 输出：1
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/shortest-subarray-with-sum-at-least-k
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

计算出前缀和之后，在暴力解法的基础上进行优化，因为要求和大于K且最短的子数组，维持一个单调递增的前缀和队列，基于队列来计算最优解。

```java
class Solution {
    public int shortestSubarray(int[] A, int K) {
        long[] preSum = new long[A.length + 1];
        for (int i = 0; i < A.length; i++) {
            preSum[i + 1] = preSum[i] + A[i];
        }

        int[] deQueue = new int[preSum.length];
        int head = 0, tail = 0;
        int ans = preSum.length;
        for (int i = 0; i < preSum.length; i++) {
            while (head < tail && preSum[i] - preSum[deQueue[head]] >= K) {
                ans = Math.min(ans, i - deQueue[head++]);
            }
            while (tail > head && preSum[deQueue[tail - 1]] >= preSum[i]) {
                tail--;
            }
            deQueue[tail++] = i;
        }
        return ans == preSum.length ? -1 : ans;

    }

}
```

