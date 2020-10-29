> 给定一个 n x n 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 k 小的元素。
> 请注意，它是排序后的第 k 小元素，而不是第 k 个不同的元素。
>
>  
>
> 示例：
>
> ```
> matrix = [
>    [ 1,  5,  9],
>    [10, 11, 13],
>    [12, 13, 15]
> ],
> k = 8,
> 
> 返回 13。
> ```
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int len = matrix.length;
        int lowBound = matrix[0][0], upperBound = matrix[len - 1][len - 1];
        while (lowBound < upperBound) {
            int mid = (lowBound + upperBound) >> 1;
            if (check(matrix, k, mid)) {
                upperBound = mid;
            } else {
                lowBound = mid + 1;
            }
        }
        return upperBound;
    }

    private boolean check(int[][] matrix, int k, int x) {
        int len = matrix.length;
        int cnt = 0;
        int row = len - 1, col = 0;
        while (row >= 0 && col < len) {
            if (matrix[row][col] <= x) {
                cnt += row + 1;
                col++;
            } else {
                row--;
            }
        }
        return cnt >= k;
    }
}
```

