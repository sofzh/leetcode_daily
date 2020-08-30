[二叉树直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)   
分析：二叉树直径是两个节点之间的最大距离，因此需要用深度优先搜索，找到每个节点的深度，然后最大直径应该在一个节点（应该就是根节点）的左儿子深度+右儿子深度+1（自身）-1（距离应该是节点数-1），这样就可以得到该二叉树的直径了    
```python3 
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        self.ans = 1
        def depth(node):
            if node is None:
                return 0 ##  空节点
            L = depth(node.left)  ## 该节点的左子树深度
            R = depth(node.right)  ## 该节点的右子树深度
            ## 更新 ans
            self.ans = max(self.ans, L + R + 1)
            ## 该节点的深度
            return max(L, R) + 1
        depth(root)
        return self.ans - 1
        
```
