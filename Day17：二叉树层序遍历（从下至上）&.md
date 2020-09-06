[二叉树层序遍历（由下至上）](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)    
分析：这里可以用广度优先搜索，遍历二叉树的每一层的节点，然后按照以前层序遍历的思路，一层一层输出节点数值（val），但是由于这个是需要从下至上进行输出结果，因此可以将结果进行反序，但是每一层的结果不用进行反序，这里用deque保存遍历到的节点，然后访问该层节点的时候，将该节点的儿子节点加入deque中，直至下一层已经没有节点可以继续访问下去。    
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        res_list = []
        if root is None:
            return res_list
        
        q = collections.deque([root])
        while q:
            level = list() ## 该层的输出
            q_len = len(q)
            ###   开始遍历每一层的节点，注意这时q保存的是该层节点，在遍历的同时要将下一层节点按照顺序存储，
            ###   因此这里需要记录遍历的个数， 也就是 popleft 的次数
            for _ in range(q_len):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            res_list.append(level)

        ## 输出时需要反序
        return res_list[::-1]

```
