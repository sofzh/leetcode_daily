[平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/submissions/)
分析：
  1、最直观的想法：前序遍历，每次判断是否符合条件，但是对于同一个节点，会计算多次高度值，因此不是很好的解法
  2、后序遍历，从叶子处开始获取高度值，这样每个节点只需计算一次，如果已经不符合条件可以提前返回结果
  
---
方法1
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isBalanced(self, root: TreeNode) -> bool:
        if root is None:
            return True 
        return abs(self.get_height(root.left) - self.get_height(root.right)) <= 1 and self.isBalanced(root.left) and self.isBalanced(root.right)

    def get_height(self, root):
        if root is None:
            return 0
        return max(self.get_height(root.left), self.get_height(root.right)) + 1
```
