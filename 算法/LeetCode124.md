> 给定一个非空二叉树，返回其最大路径和。
>
> 本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/binary-tree-maximum-path-sum

计算根节点，根节点加上左子树上的路径，根节点加上右子树上的路径这三个的最大值，就是跨过某个节点的的最大路径和。后续遍历整个数计算每个节点的最大路径和，返回最大值。

```java
class Solution {
    private int ans = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        maxPath(root);
        return ans;
    }
	
    private int maxPath(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = maxPath(root.left);
        int r = maxPath(root.right);
        int res = Math.max(root.val, Math.max(l, r) + root.val);
        ans = Math.max(ans, res);
        ans = Math.max(ans, l + r + root.val);
        return res;
    }
}
```

