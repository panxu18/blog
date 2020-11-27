> 给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。
>
> 来自 leetcode
>
> https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }
    ArrayList<TreeNode> que = new ArrayList<>();
    que.add(root);
    int level = 0;
    while (!que.isEmpty()) {
        ArrayList<TreeNode> nextQue = new ArrayList<>();
        ArrayList<Integer> resLine = new ArrayList<>();
        for (TreeNode node : que) {
            resLine.add(node.val);
            if (node.left != null) {
                nextQue.add(node.left);
            }
            if (node.right != null) {
                nextQue.add(node.right);
            }
        }
        que = nextQue;
        if ((level++&1) == 1) {
            Collections.reverse(resLine);
        }
        res.add(resLine);
    }
    return res;
}
```

