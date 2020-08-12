# LeetCode Daily
写在前面的话：   
&emsp;&emsp; 记录刷题过程，鞭策自己    

---

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
