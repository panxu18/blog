> 给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。
>
> 岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。
>
> 此外，你可以假设该网格的四条边均被水包围。
>
> **示例 1：**
>
> ```
> 输入：grid = [
>   ["1","1","1","1","0"],
>   ["1","1","0","1","0"],
>   ["1","1","0","0","0"],
>   ["0","0","0","0","0"]
> ]
> 输出：1
> ```
>
> 
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/number-of-islands
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    int[][] direct = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    public int numIslands(char[][] grid) {
        int cnt = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    cnt++;
                }
            }
        }
        return cnt;
    }

    private void dfs(char[][] grid, int x, int y) {
        grid[x][y] = '0';
         for (int i = 0; i < direct.length; i++) {
             int nx = direct[i][0] + x;
             int ny = direct[i][1] + y;
             if (nx >= 0 && nx < grid.length
                && ny >= 0 && ny < grid[0].length
                && grid[nx][ny] == '1') {
                dfs(grid, nx, ny);
            }
         }
    }
}
```

