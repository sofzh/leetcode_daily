[平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/submissions/)   

分析：   

&emsp; 1、最直观的想法：前序遍历，每次判断是否符合条件，但是对于同一个节点，会计算多次高度值，因此不是很好的解法   

&emsp; 2、后序遍历，从叶子处开始获取高度值，这样每个节点只需计算一次，如果已经不符合条件可以提前返回结果   

  
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
---   
方法2   
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
        return self.get_height(root) >= 0

    def get_height(self, root):
        if root is None:
            return 0
        h_l = self.get_height(root.left)
        if h_l == -1:
            return -1
        h_r  = self.get_height(root.right)
        if h_r == -1:
            return -1
        if abs(h_l - h_r) > 1:
            return -1
        return max(h_l, h_r) + 1

```
   
---   
[二叉搜索树验证](https://leetcode-cn.com/problems/validate-binary-search-tree/)   
分析：通过中序遍历获得一个排好序的list，如果是二叉搜索树，那么得到的list应该是个升序序列，或者在中序遍历的同时进行判断（注意中序遍历循环的终止判断条件）   

```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isValidBST(self, root: TreeNode) -> bool:
        if root is None:
            return True 
        ## 中序遍历
        stack, min_v = [], float('-inf')
        while stack or root:
            ## 中序遍历， stack存访问的节点
            ### 优先访问左儿子
            while root:
                stack.append(root)
                root = root.left
            ### 这时的root已经没有左儿子, 开始取出父节点
            root = stack.pop()
            if root.val <= min_v:
                ## 此时父节点的值 <= min_v 说明不是搜索树
                return False 
            min_v = root.val 
            # 更新 min_v 开始访问这时root的右儿子
            root = root.right 

        return True 
            
```
[两数之和](https://leetcode-cn.com/problems/two-sum/)    
分析：第一个思路是排序然后查找这俩数， 如果后面的查找（从大到小）没找到，说明不存在，第二个思路是借助dict判断缺的数是否是在dict中，要注意一些细节，题目中说一个数不能多次使用，但是没说给的数中是不是有等值得数   
```python3
class p_tmp(object):
    def __init__(self):
        self.val = None
        self.idx = None
class Solution(object):
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        res = None 
        if nums == []:
            return res  
        dic = {}
        
        for idx, i in enumerate(nums):
            if i not in dic:
                dic[i] = p_tmp()
                dic[i].val = 1
                dic[i].idx = [idx]
            else: 
                dic[i].val += 1
                dic[i].idx.append(idx)
        for idx, i in enumerate(nums):
            j = target - i 
            if j in dic:
                if i == j:
                    dic[j].val -= 2
                    if dic[j].val >= 0:
                        return [dic[j].idx[0], dic[j].idx[1]]
                    else:
                        continue 
                else:
                    return [dic[i].idx[0], dic[j].idx[0]]
            else:
                continue  
        return False 
```
