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
