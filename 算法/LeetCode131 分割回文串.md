> 给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。
>
> 返回 s 所有可能的分割方案。
>
> 示例:
>
> 输入: "aab"
> 输出:
>
> ```
> 输入: "aab"
> 输出:
> [
>   ["aa","b"],
>   ["a","a","b"]
> ]
> ```
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/palindrome-partitioning
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public List<List<String>> partition(String s) {
        int len = s.length();
        char[] arr = s.toCharArray();
        boolean[][] isPalindromes = new boolean[len][len];
        for (int i = len - 1; i >= 0; i--) {
            for (int j = i; j < len; j++) {
                isPalindromes[i][j] = arr[i] == arr[j] && (j - i <= 1 || isPalindromes[i + 1][j - 1]);
            }
        }
        List<List<String>> result = new ArrayList<>();
        ArrayList<String> stack = new ArrayList<>();
        dfs(arr, isPalindromes, result, stack, 0);
        return result;
    }

    private void dfs(char[] arr, boolean[][] isPalindromes, List<List<String>> result, 
        ArrayList<String> stack, int depth) {
        if (depth == arr.length) {
            result.add(new ArrayList<>(stack));
            return;
        }
        for (int i = depth; i < arr.length; i++) {
            if (isPalindromes[depth][i]) {
                stack.add(String.valueOf(arr, depth, i - depth + 1));
                dfs(arr, isPalindromes, result, stack, i + 1);
                stack.remove(stack.size() - 1);
            }
        }
    }
}
```

