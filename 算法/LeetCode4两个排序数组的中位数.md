> 给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。
>
> 进阶：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗？
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/median-of-two-sorted-arrays

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums2.length < nums1.length) {
            return findMedianSortedArrays(nums2, nums1);
        }
        int k = (nums1.length + nums2.length + 1) >> 1;
        int lb = 0, ub = nums1.length;
        
        while (lb < ub) {
            int m1 = (lb + ub) >> 1;
            int m2 = k - m1;
            if (nums1[m1] < nums2[m2 - 1]) {
                lb = m1 + 1;
            } else{
                ub = m1;
            }
        }
        int m1 = ub;
        int m2 = k - m1;
        int left = m1 == 0 ? nums2[m2 - 1] : m2 == 0 ? nums1[m1 - 1] : Math.max(nums1[m1 - 1], nums2[m2 - 1]);
        if (((nums1.length + nums2.length) & 1) == 1) {
            return left;
        }
        int right = m1 == nums1.length ? nums2[m2] : m2 == nums2.length ? nums1[m1] : Math.min(nums1[m1], nums2[m2]);
        return 0.5 * (left + right);
    }
}
```

