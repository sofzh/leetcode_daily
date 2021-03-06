[打家劫舍](https://leetcode-cn.com/problems/house-robber/)    
分析：DP问题，n家可以偷的最大价值 = max（n-1家可以偷的最大值， nums[n] + n-2家可以偷的最大价值）具体分析可以看代码里面       
```python3
class Solution:
    def rob(self, nums: List[int]) -> int:
        def rob_n(n, curr, prev):
            if n == 0:
                return nums[0]
            ## DP问题 ： 这时有两种选择，h(n) = max(h(n-1), h(n-2) + nums[n])
            ## h(n) 代表有n家可以偷的最大价值 
            ## 1.选择偷n， 那么应该是，之前最多偷n-2家
            ## 2.选择不偷n，就相当于最多偷n-1家
            res = max(curr, prev + nums[n])
            
            return res
        len_l = len(nums)
        prev = 0
        curr = nums[0] if len_l > 0 else 0
        res = curr
        for i in range(1, len_l): ## 因为这里从1开始，所以 res 有一个初始值 curr：nums[0]
            res = rob_n(i, curr, prev)
            curr, prev = res, curr
        return res
```
更好的答案，更简约，我的思路有点乱了。。。。    
```python3
def rob(self, nums: List[int]) -> int:
    prev = 0
    curr = 0
    
    # 每次循环，计算“偷到当前房子为止的最大金额”
    for i in nums:
        # 循环开始时，curr 表示 dp[k-1]，prev 表示 dp[k-2]
        # dp[k] = max{ dp[k-1], dp[k-2] + i }
        prev, curr = curr, max(curr, prev + i)
        # 循环结束时，curr 表示 dp[k]，prev 表示 dp[k-1]

    return curr

作者：nettee
链接：https://leetcode-cn.com/problems/house-robber/solution/dong-tai-gui-hua-jie-ti-si-bu-zou-xiang-jie-cjavap/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
---    
[镜像二叉树](https://leetcode-cn.com/problems/symmetric-tree/)    
分析：方法1：可以递归的判断左子树与右子树是否是镜像对称的，递归终止条件：递归的终止情况与返回值：（1）左右子树都为空 → True （2）左右子树一个为空 → False （3）左右子树都不空，但是值不相等 → False （4）若上述情况都不满足， 检查 左左&右右， 左右&右左， 方法2：也可以采用层序遍历，每层进行比较是否对称    
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        def is_symmetric(left, right):
            if left is None and right is None:
                return True 
            if left is None or right is None:
                return False
            if left.val != right.val:
                return False
            return is_symmetric(left.left, right.right) and is_symmetric(left.right, right.left)
        return is_symmetric(root.left, root.right)

        
            

```
