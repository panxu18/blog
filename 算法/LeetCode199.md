> 给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/binary-tree-right-side-view/

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        ArrayList<TreeNode> que = new ArrayList<>();
        que.add(root);
        while (!que.isEmpty()) {
            res.add(que.get(que.size()- 1).val);
            ArrayList<TreeNode> next = new ArrayList<>();
            for (TreeNode node : que) {
                if (node.left != null) {
                    next.add(node.left);
                }
                if (node.right != null) {
                    next.add(node.right);
                }
            }
            que = next;
        }
        return res;
    }
}
```

