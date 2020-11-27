> 给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。
>
>  
>
> 提示：
>
> num1 和num2 的长度都小于 5100
> num1 和num2 都只包含数字 0-9
> num1 和num2 都不包含任何前导零
> 你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/add-strings

```java
class Solution {
    public String addStrings(String num1, String num2) {
        StringBuilder builder = new StringBuilder();
        char[] chars1 = num1.toCharArray();
        char[] chars2 = num2.toCharArray();
        int i = chars1.length - 1;
        int j = chars2.length - 1;
        int carry = 0;
        while (i >= 0 || j >= 0 || carry > 0) {
            int x = i < 0 ? 0 : chars1[i--] - '0';
            int y = j < 0 ? 0 : chars2[j--] - '0';
            int sum = x + y + carry;
            builder.append(sum % 10);
            carry = sum >= 10 ? 1 : 0;
        }
        return builder.reverse().toString();
    }
}
```

