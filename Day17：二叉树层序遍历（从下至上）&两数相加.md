[二叉树层序遍历（由下至上）](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)    
分析：这里可以用广度优先搜索，遍历二叉树的每一层的节点，然后按照以前层序遍历的思路，一层一层输出节点数值（val），但是由于这个是需要从下至上进行输出结果，因此可以将结果进行反序，但是每一层的结果不用进行<font color=red> 反序 </font>，这里用deque保存遍历到的节点，然后访问该层节点的时候，将该节点的儿子节点加入deque中，直至下一层已经没有节点可以继续访问下去。    
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

[两数相加](https://leetcode-cn.com/problems/add-two-numbers/)    
分析：   
&emsp; 1.模仿两数竖式相加的过程（因为所有数是非负的，所以只需要考虑非负数的加法，不用考虑减法），需要考虑进位（这时进位只能是0或者1），程序中用extra表示是否有进位，默认是0，两数相加因为有进位的存在，因此需要考虑进位的加和，比如 1+ 999 = 1000将产生一个4位数，进位一直在进直至最后一位，这里实现的时候需要判断进位是否为1，   
&emsp; 2.最终两个链表都访问结束后，需要判断这时是否有进位，就像1+999=1000一样，结果是4位的，最高位9与进位1相加得到一个长度更长的链表，因此实现的时候需要遍历两个链表，直至两个链表都遍历完毕才算完成相加的过程，循环结束的条件是两个链表都访问完毕，   
&emsp; 3.循环未完成时（至少一个链表未完全访问时），结果链表的下一位需要初始化，不然下一位是None将无法访问val属性值，这里是一个需要考虑的细节。     
```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1.val == 0 and l1.next is None:
            return l2 
        if l2.val == 0 and l2.next is None:
            return l1 
        res = ListNode(0)
        tmp = res
        extra = 0
        while l1 is not None or l2 is not None:
            if l1 and l2:
                tmp.val = l1.val + l2.val + extra 
            elif l1:
                tmp.val = l1.val + extra 
            else:
                ## l2 
                tmp.val = l2.val + extra 
            if tmp.val >= 10:
                extra = 1
                tmp.val = tmp.val % 10
            else:
                extra = 0 ##  万一之前的extra 是 1 这里需要对 extra 重置为0
            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next
           
            ## 结束条件：两个链表都完全访问 否则将继续访问未访问完的链表
            if l1 is None and l2 is None:
                if extra == 1:
                    ## 如果有进位， 否则没有下一位，即下一位是None
                    tmp.next = ListNode(extra)
                break  
            else:
                ## 默认下一位 是 0开始 ， 如果是 None 将无法访问 下一个的 val
                tmp.next = ListNode(0)
                tmp = tmp.next 
                    
        return res
```
