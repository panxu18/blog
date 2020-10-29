> 给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
>
> 求在该柱状图中，能够勾勒出来的矩形的最大面积。
>
> 示例:
>
> ```
> 输入: [2,1,5,6,2,3]
> 输出: 10
> ```
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/largest-rectangle-in-histogram
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int[] stack = new int[heights.length + 1];
        int top = 0;
        int ans = 0;
        for (int i = 0; i < heights.length; i++) {
            while (top > 0 && heights[i] <= heights[stack[top - 1]]) {
                int idx = stack[--top];
                int left = top == 0 ? -1 : stack[top - 1];
                ans = Math.max(ans, heights[idx] * (i - left - 1));
            }
            stack[top++] = i;
        }
        while (top > 0) {
            int idx = stack[--top];
            int left = top == 0 ? -1 : stack[top - 1];
            ans = Math.max(ans, heights[idx] * (heights.length - left - 1));
        }
        return ans;
    }
}
```

