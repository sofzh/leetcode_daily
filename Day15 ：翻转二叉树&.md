[翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
分析：大致有两种思路，1.采用递归，递归交换节点的左右儿子， 2.用队列，对于每一层节点进行访问，并交换左右儿子，然后一直至叶节点     
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if root is None:
            return root 
        tmp_l, tmp_r = root.left, root.right
        root.right, root.left = tmp_l, tmp_r
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```

---     

