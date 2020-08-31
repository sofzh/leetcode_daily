[多数元素](https://leetcode-cn.com/problems/majority-element/)     
分析：这里多数元素是个数超过一半的元素，因此第一种方法可以用哈希表保存元素个数，选择大于半数的即可，第二种方法可以借助排序，因为这里多数元素>一半因此中间值肯定是我们要找的元素，第三种方法，Boyer-Moore 投票算法，因为这里的众数是大于一半的， 因此采取这种投票，用candidate表示选择的众数，count表示目前该数出现的比其他数多的次数，当count==0时，更新candidate，最终遍历数组，最后剩下的candidate才是我们要的众数，这里count是众数比其他数多的个数，一定是非负的    
```python3 
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = None

        for num in nums:
            if count == 0:
                candidate = num 
            count += (1 if candidate == num else -1)

        return candidate
```
----    
[买卖股票最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)     
分析：这是一个dp（动态规划问题），这里维护两个值，一个是当前最小买入价格min_val，一个是当前最大利润max_diff，从头遍历数组，最大利润应该是 一个大数 - 一个小数， 不一定是 max_val - min_val，因为买入卖出需要符合客观时间规律，所以我们一边遍历数组，一边更新最小值min_val， 一边维护当前最大利润max_diff， 直至遍历完数组， 我们即可得到最大利润 max_diff， 详情可见code实现，里面记得比较详细        
```python3
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        ## dp 更新 min_val 维护 max_diff
        max_diff = 0
        if prices == []:
            return max_diff
        min_val = prices[0]
        len_p = len(prices)
        for idx in range(1, len_p):
            ## 如果找到了更小的min_val 那么更新min_val
            if prices[idx] < min_val:
                min_val = prices[idx]
            ## 注意： 因为卖的时间需要比买的时间晚，在买时间之后，因此之前的最大利润不一定及时更新，
            ## 需要在后面找到差值更大的时候才可以更新，所以当不更新min_val的时候遇见的值都是比目前的min_val更大的值，
            ## 这个时候再去判断当前price - min_val 会不会比 当前的 max_diff 更大更为合理
            elif prices[idx] - min_val > max_diff:
                max_diff = prices[idx] - min_val
            
        return max_diff


```
