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
[除法求值](https://leetcode-cn.com/problems/evaluate-division/)    
分析：这个题目需要用并查集的思路去做，将有关系的因子放在一个并查集中，根据是否具有同一个根判断是否在一个并查集中， 除法相当于在一个并查集中，由被除数至除数之间的权重计算，具体可见code实现     
```python3
from collections import namedtuple, defaultdict

class Solution:
    def calcEquation(self, equations, values, queries) :
        class Item:
            def __init__(self, parent, value):
                self.parent = parent
                self.value = value

        father = defaultdict(Item)

        def find(x):
        #返回值为根节点
        #如果它是独立元素，根节点是自身，返回自己。不然，递归找到根节点，用路径压缩修改parent，并返回根节点。
            if x != father[x].parent:
                t = father[x].parent
                father[x].parent = find(t)
                father[x].value *= father[t].value  #1---3的权重就是（1--2的权重）*（2---3的权重）
                return father[x].parent
            return x 

        def union(e1, e2, result):
            # 平行四边形法则边 (e1, e2), (e1, f1), (e2, f2) --> (f1, f2)
            #找到根节点判断是否相等
            f1 = find(e1)
            f2 = find(e2)
            #分别属于两个集合，需要合并：
            if f1 != f2:
                father[f1].parent = f2  #第一个根节点指向第二个根节点
                #更新权重，具体方法通过上面的例子可以算一下
                father[f1].value = father[e2].value * result / father[e1].value

        #用字典提高效率，判断是否见过
        number = defaultdict(int)

        #初始化：每个节点的父节点初始化为自身，不指向任何人，value为1
        for i,item in enumerate(equations) :
            s1, s2 = item[0], item[1]
            if s1 not in number:
                number[s1] =  1
                father[s1] = Item(parent=s1, value=1)
            if s2 not in number:
                number[s2] =  1
                father[s2] = Item(parent=s2, value=1)
            #根据两个节点的关系，合并两个节点。构建tree
            union(s1, s2, values[i])

        #开始计算：
        ans = []
        for s1, s2 in queries:
            #从来没见过，直接-1
            if s1 not in number or s2 not in number:
                ans.append(-1.0)
                continue
            #找到根节点，如果不想等，说明在不同集合tree中，没有任何关系，直接-1
            e1, e2 = s1, s2
            f1, f2 = find(e1), find(e2)
            if f1 != f2:
                ans.append(-1.0)
            else:
                #在同一个tree中
                v1 = father[e1].value
                v2 = father[e2].value
                #v1=e1/root   v2=e2/root  则e1/e2=v1/v2
                ans.append(v1 / v2)
        return ans



```
