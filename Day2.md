1、[汉明距离](https://leetcode-cn.com/problems/hamming-distance/)
分析：计算两个整数二进制表示的不同位置的数目，即这两个整数二进制异或的结果中1的个数，这里借鉴leetcode中一个巧妙的算法：布赖恩·克尼根算法，利用 n & (n-1)将消去n中最右的1，其中n>0，这样直至n==0，即可将两个数的异或结果中的所有1求出来
```python3
class Solution(object):
    def hammingDistance(self, x: int, y: int) -> int:
        # return bin(x^y).count('1')
        xor = x ^ y
        dist = 0
        while xor:
            dist += 1
            xor = xor & (xor - 1) ## 去掉 xor 最右边的 1

        return dist
```
