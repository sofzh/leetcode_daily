[有序链表转二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)   
分析：这个有序链表刚好是这个搜索树的中序遍历结果，因此可以考虑用中序遍历的方式构造出这个二叉搜索树   
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
