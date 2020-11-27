> 给定一个区间的集合，找到需要移除区间的最小数量，使剩余区间互不重叠。
>
> **注意:**
>
> 1. 可以认为区间的终点总是大于它的起点。
> 2. 区间 [1,2] 和 [2,3] 的边界相互“接触”，但没有相互重叠。
>
>
> **示例 1:**
>
> ```
> 输入: [ [1,2], [2,3], [3,4], [1,3] ]
> 
> 输出: 1
> 
> 解释: 移除 [1,3] 后，剩下的区间没有重叠。
> ```
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/non-overlapping-intervals
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, Comparator.comparingInt(arr -> arr[0]));
        int lastEnd = Integer.MIN_VALUE;
        int cnt = 0;
        for (int[] interval : intervals) {
            if (interval[0] < lastEnd) {
                lastEnd = Math.min(lastEnd, interval[1]);
                cnt++;
            } else {
                lastEnd = interval[1];
            }
        }
        return cnt;
    }
}
```

