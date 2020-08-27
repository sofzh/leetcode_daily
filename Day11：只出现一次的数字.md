[只出现一次的数字](https://leetcode-cn.com/problems/single-number/)    
分析：如果题目中不限制 “不适用额外空间” 这条， 解决问题的思路有很多， 比如用dict 记录出现的次数，[题解](https://leetcode-cn.com/problems/single-number/solution/zhi-chu-xian-yi-ci-de-shu-zi-by-leetcode-solution/)中提到了 “异或”运算， 之前没有想到，这里异或运算的性质比较重要    
1、任何数和 0 做异或运算，结果仍然是原来的数    
2、任何数和其自身做异或运算，结果是 0    
3、异或运算满足交换律和结合律    
这里直接用这三条性质做就会比较简单，能想到异或就简单，想不到就会比较难    
```python3 
from functools import reduce
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        return reduce(lambda x, y: x ^ y, nums)
```
