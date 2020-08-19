[回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)    
计算字符串中回文子串的个数   
分析：第一开始想着是暴力求解，虽然可以得到正确答案，但是太过费时，时间是O(n^3)，果然无法通过，后看解答，发现先找回文中心，然后向两边扩展，而不是由左至右挨个计算，这样的话时间复杂度是O(n^2)，因为一个字符串的回文中心可能在字符上也有可能在俩字符中间，因此回文中心一共有2n-1个（n：字符串长度）   
```python3
class Solution:
    def countSubstrings(self, s: str) -> int:
        if s == '':
            return 0 
        n = len(s)
        res = 0
        for i in range(2 * n - 1):
            l = i >> 1
            # r = i >> 1 + i % 2 ## 这样写有问题 因为 i 已经右移了
            r = i % 2 + i >> 1
            
            ## 回文中心 左中心 l 右中心 r 
            while l >= 0 and r < n and s[l] == s[r]:
                res += 1
                ### 由中心向两边扩展
                # print(f'i : {i}, l :  {l}, r : {r}')
                # print(s[l:r+1])
                l = l - 1
                r = r + 1
        return res 
```
