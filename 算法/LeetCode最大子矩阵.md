> 给定一个正整数、负整数和 0 组成的 N × M 矩阵，编写代码找出元素总和最大的子矩阵。
>
> 返回一个数组 [r1, c1, r2, c2]，其中 r1, c1 分别代表子矩阵左上角的行号和列号，r2, c2 分别代表右下角的行号和列号。若有多个满足条件的子矩阵，返回任意一个均可。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/max-submatrix-lcci
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public int[] getMaxMatrix(int[][] matrix) {
        int row = matrix.length;
        int col = matrix[0].length;
        for (int i = 1; i < row; i++) {
            for (int j = 0; j < col; j++) {
                matrix[i][j] += matrix[i - 1][j];
            }
        }
        int ans = Integer.MIN_VALUE;
        int[] res = new int[4];
        for (int i = 0; i < row; i++) {
            for (int j = row - 1; j >= i; j--) {
                int dp = 0;
                int left = 0;
                for (int k = 0; k < col; k++) {
                    dp += matrix[j][k];
                    if (dp < matrix[j][k]) {
                        left = k;
                        dp = matrix[j][k];
                    }
                    if (dp > ans) {
                        ans = dp;
                        res[0] = i;
                        res[1] = left;
                        res[2] = j;
                        res[3] = k;
                    }
                    if (j > i) {
                        matrix[j][k] -= matrix[i][k];
                    } 
                }  
            }
        }
        return res;
    }
}
```

