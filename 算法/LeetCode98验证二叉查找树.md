> 给定一个二叉树，判断其是否是一个有效的二叉搜索树。
>
> 假设一个二叉搜索树具有如下特征：
>
> 节点的左子树只包含小于当前节点的数。
> 节点的右子树只包含大于当前节点的数。
> 所有左子树和右子树自身必须也是二叉搜索树。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/validate-binary-search-tree

```java
class Solution {
    private long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        return (root == null) 
        || isValidBST(root.left)
        && pre != root.val && (pre = Math.max(pre, root.val)) == root.val
        && isValidBST(root.right);
        
    }

}
```

