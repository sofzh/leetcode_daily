[机器人返回原点](https://leetcode-cn.com/problems/robot-return-to-origin/)    
分析：这里有两个思路，因为机器人只能移动4个方向，1.模拟机器人移动，用xy坐标记录机器人的位置，如果最后机器人坐标为原点，则说明可以返回； 2.无论机器人怎么移动在x、y方向上只要分别保持左右移动次数一致、上下移动次数一致就可以保证最终回到了原点，这里可以将x、y方向分别考虑，就像计算物体的位移一样（这个思路没有写code验证），这里给出方案1的code    
```python3
class Solution:
    def judgeCircle(self, moves: str) -> bool:
        pos = [0, 0]
        if moves == '':
            return True
        for str_i in moves:
            if str_i == 'U':
                pos[1] += 1
            elif str_i == 'D':
                pos[1] -= 1
            elif str_i == 'L':
                pos[0] -= 1;
            else:
                pos[0] += 1;
        return pos == [0, 0]
```

---    
[环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)    
分析：判断环形链表一般都是采用快慢指针方法，如果有环，那么一定这俩指针在后面会相遇，否则就没环， 如果这里需要判断环开始的位置的话，就需要用hash表记录访问过的节点了，这里只是判断有无环，直接上快慢指针就可以了，不过这里要注意fast指针因为一次移动两次，这里要提前判断能否移动两次         
```python3 
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if head is None or head.next is None:
            return False
        slow, fast = head, head.next 
        while slow != fast:
            if fast is None or fast.next is None: ## 因为移动两次 所以提前判断 是否可以移动两次， 注意这个细节
                return False 
            slow = slow.next 
            fast = fast.next.next 
        return True 

```
