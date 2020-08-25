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
