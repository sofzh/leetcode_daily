1、[克隆图（无向图）][https://leetcode-cn.com/problems/clone-graph/]
分析：首先输入只是一个图的Node， 需要边便利Node， 边进行深copy， 采用DFS策略， 需要注意需要记录已访问节点，不然容易陷入死循环，用递归的方式进行DFS，邻接列表也需要重头构建
```python3
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = []):
        self.val = val
        self.neighbors = neighbors
"""

class Solution(object):

    def __init__(self):
        self.visited = {}

    def cloneGraph(self, node: 'Node') -> 'Node':
        if not node:
            return node 
        
        # 如果 Node 已经访问过， 直接返回 clone的节点 Node
        if node in self.visited:
            return self.visited[node]
        
        # 如果还没访问， 就创建 clone Node
        clone_node = Node(node.val, neighbors=[])  ## 深拷贝， 这里需要重头创建 neighbors
        self.visited[node] = clone_node  ## 建立原始 Node 到 clone Node 的映射

        if node.neighbors:
            # 邻接列表
            clone_node.neighbors = [self.cloneGraph(node_i) for node_i in node.neighbors]
        
        return clone_node
```

---

2、[二叉树深度][https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/]
分析：运用递归的思想：max(root) = max(l, r) + 1, 因此只需递归的找到左二子跟右儿子最大深度+1(自己本身)，只需
给出递归终止条件--节点为None的时候，返回即可

```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxDepth(self, root: TreeNode) -> int:
        depth = 0
        if not root:
            return depth
        depth = 1
        if root.left or root.right:
            depth += max(self.maxDepth(root.left), self.maxDepth(root.right))
        return depth
            
```
