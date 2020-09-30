> 给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/trapping-rain-water

横向考虑两个柱子之间的容量，从左到右对每个谷进行计算，为了不重复计算，计算过的谷需要移除。模拟这个过程就会发现形成一个单调递减栈。

```java
public int trap(int[] height) {
        int sum = 0;
        int[] left = new int[height.length];
        int top = 0;
        for (int i = 0; i < height.length; i++) {
            while (top > 0 && height[left[top - 1]] <= height[i]) {
                if (top-- > 1) {
                    sum += (Math.min(height[left[top - 1]],height[i]) - height[left[top]] ) * (i - left[top - 1] - 1);
                }
            }
            left[top++] = i;
        }
        return sum;
    }
```



