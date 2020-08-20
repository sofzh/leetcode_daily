[验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)    
分析：这里只考虑字符或者数字，因此可以借助 str.isalnum()来判断是否是属于字符或者数字，然后用双指针，从两头向中间遍历，直到两指针相遇或者相交，说明是回文，中间若是有不符合情况的，说明是非回文串   
```python3
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s_str = ''.join(ch.lower() for ch in s if ch.isalnum())
        n = len(s_str)
        l, r = 0, n-1
        while l < r:
            if s_str[l] != s_str[r]:
                return False
            l += 1
            r -= 1
        return True
```
[两数之和](https://leetcode-cn.com/problems/two-sum-iii-data-structure-design/)   
分析：这里采用hash的方式，存储已有的数据，然后判断需要find的数是不是在已存的dict里面，要注意如果两个数字相同，则需要知道这个数字有多少个，如果>1则说明是找到了两个数之和==要find 的数字    
```python3
class TwoSum:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.num = {}


    def add(self, number: int) -> None:
        """
        Add the number to an internal data structure..
        """
        if number not in self.num:
            self.num[number] = 0
        else:
            self.num[number] += 1


    def find(self, value: int) -> bool:
        """
        Find if there exists any pair of numbers which sum is equal to the value.
        """
        for k in self.num:
            k_pair = value - k 
            if k_pair == k:
                if self.num[k] > 0:
                    return True 
                else:
                    continue
            if k_pair in self.num:
                return True 
        return False



# Your TwoSum object will be instantiated and called as such:
# obj = TwoSum()
# obj.add(number)
# param_2 = obj.find(value)
```
