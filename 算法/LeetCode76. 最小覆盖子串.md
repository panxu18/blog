> 给你一个字符串 S、一个字符串 T 。请你设计一种算法，可以在 O(n) 的时间复杂度内，从字符串 S 里面找出：包含 T 所有字符的最小子串。
>
>  
>
> 示例：
>
> 输入：S = "ADOBECODEBANC", T = "ABC"
> 输出："BANC"
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/minimum-window-substring
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] map = new int[128];
        int total = 0;
        for (char c : t.toCharArray()) {
            if (map[c]++ == 0) {
                total++;
            }
        }
        char[] arr = s.toCharArray();
        int rangeLeft = 0, rangeRight = -1;
        int head = 0, tail = 0, cnt = 0;
        for (; tail < arr.length; tail++) {
            if (--map[arr[tail]] == 0) {
                cnt++;
            }
            while (cnt == total) {
                if (rangeLeft > rangeRight || tail - head < rangeRight - rangeLeft) {
                    rangeLeft = head;
                    rangeRight = tail;
                }
                if (map[arr[head++]]++ == 0) {
                    cnt--;
                }
            }
        }
        return String.valueOf(arr, rangeLeft, rangeRight - rangeLeft + 1);
    }
}
```

