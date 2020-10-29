> 给定一个二进制数组, 找到含有相同数量的 0 和 1 的最长连续子数组（的长度）。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/contiguous-array

```java
class Solution {
    public int findMaxLength(int[] nums) {
        int count = 0;
        int max = 0;
        int offset = nums.length;
        int[] index = new int[(nums.length << 1) + 1];
        for (int i = 0; i < nums.length; i++) {
            count += nums[i] == 1 ? 1 : -1;
            if (count == 0 || index[count + offset] != 0) {
                max = Math.max(i - index[count + offset] + 1, max);
            } else{
                index[count + offset] = i + 1;
            }
        }
        return max;
    }
}
```

