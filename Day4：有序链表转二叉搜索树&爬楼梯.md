[有序链表转二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)   
分析：这个有序链表刚好是这个搜索树的中序遍历结果，中间的值即为根节点，可以这样一步一步构造出来二叉树，因此可以考虑用中序遍历的方式构造出这个二叉搜索树   
```python3 
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def get_length(head : ListNode) -> int:
            h = 0
            while head:
                h += 1
                head = head.next
            return h 
        
        def build_tree(l : int, r : int) -> TreeNode:
            if l > r:
                return None
            mid = (l + r + 1) // 2
            root = TreeNode()
            root.left = build_tree(l, mid-1)
            nonlocal head ## 这里需要声明 nonlocal
            root.val = head.val 
            head = head.next 
            root.right = build_tree(mid+1, r)
            return root 
        length = get_length(head)
        return build_tree(0, length-1)
```

[爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)   
分析：这里是斐波那契数列， 直接维护3个值，一直递增下去即可，用动态规划的方法（看到题解里面各种数学推导真是汗颜，没想到还有这么多用数学推公式的方法。。。[官方题解](https://leetcode-cn.com/problems/climbing-stairs/solution/pa-lou-ti-by-leetcode-solution/)）    
```python3
class Solution:
    def climbStairs(self, n: int) -> int:
        p, q, r = 0, 0, 1
        for i in range(1, n+1):
            p = q
            q = r
            r = q + p
        return r 
```
