> 给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。
>
> 返回滑动窗口中的最大值。
>
>  
>
> 进阶：
>
> 你能在线性时间复杂度内解决此题吗？
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/sliding-window-maximum

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int[] ans = new int[nums.length - k + 1];
        int[] que = new int[nums.length];
        int head = 0, tail = 0;
        for (int i = 0; i < nums.length; i++) {
            if (tail - head == k|| i - que[head] == k) {
                head++;
            }
            while (tail != head && nums[que[tail - 1]] <= nums[i]) {
                tail--;
            }
            que[tail++] = i;
            if (i >= k - 1) {
                ans[i - k + 1] = nums[que[head]];
            }
        }
        return ans;
    }
}
```

