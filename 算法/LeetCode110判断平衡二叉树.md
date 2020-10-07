> 给定一个二叉树，判断它是否是高度平衡的二叉树。
>
> 本题中，一棵高度平衡二叉树定义为：
>
> 一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。
>
> 示例 1:
>
> 给定二叉树 [3,9,20,null,null,15,7]
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/balanced-binary-tree

```java
class Solution {
    private boolean flag = true;
    public boolean isBalanced(TreeNode root) {
        getHeight(root);
        return flag;
    }

    private int getHeight(TreeNode root) {
        if (!flag || root == null) {
            return 0;
        }
        int l = getHeight(root.left);
        int r = getHeight(root.right);
        if (l - r > 1 || l - r < -1) {
            flag = false;
        }
        return 1 + Math.max(l, r);
    }
}
```

