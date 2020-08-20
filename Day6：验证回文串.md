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
