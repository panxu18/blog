> 给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。
>
> 示例 1：
>
> ```text
> 输入: "babad"
> 输出: "bab"
> 注意: "aba" 也是一个有效答案。
> ```
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/longest-palindromic-substring
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public String longestPalindrome(String s) {
        char[] arr = s.toCharArray();
        int length = arr.length;
        boolean[][] dp = new boolean[length][length];
        String ans = "";
        for (int i = length - 1; i >= 0; i--) {
            for (int j = i; j < length; j++) {
                if (j == i) {
                    dp[i][j] = true;
                }else if (j - i == 1) {
                    dp[i][j] = arr[i] == arr[j];
                } else if (arr[i] == arr[j]) {
                    dp[i][j] = dp[i + 1][j - 1];
                }
                if (dp[i][j] && j - i + 1 > ans.length()) {
                    ans = new String(arr, i, j - i + 1);
                }
            }
        }
        return ans;
    }
}
```

