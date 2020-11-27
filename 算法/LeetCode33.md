> 假设按照升序排序的数组在预先未知的某个点上进行了旋转。
>
> ( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。
>
> 搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。
>
> 你可以假设数组中不存在重复的元素。
>
> 你的算法时间复杂度必须是 O(log n) 级别。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array

```java
class Solution {
    public int search(int[] nums, int target) {
        int lb = 0, ub = nums.length - 1;
        while (lb <= ub) {
            int mid = (lb + ub) >> 1;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] >= nums[0]) {
                if (target >= nums[lb] && target < nums[mid]) {
                    ub = mid - 1;
                } else {
                    lb = mid + 1;
                }
            } else {
                if (target > nums[mid] && target <= nums[ub]) {
                    lb = mid + 1;
                } else {
                    ub = mid - 1;
                }
            }
        }
        return -1;
    }
}
```

