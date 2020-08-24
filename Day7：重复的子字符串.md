[重复的子字符串](https://leetcode-cn.com/problems/repeated-substring-pattern/)    
分析：如果可以在两个s拼接而成的字符串中（除去首尾位置）找到一个与s相等的子串，那么就可以证明s是由重复的子串构建而成，详细证明请看[题解](https://leetcode-cn.com/problems/repeated-substring-pattern/solution/zhong-fu-de-zi-zi-fu-chuan-by-leetcode-solution/)    
```python3
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        ## 从1位置处开始查， 并且查询到的位置不是从n开始（即去除首尾）
        return (s + s).find(s, 1) != len(s)


```
