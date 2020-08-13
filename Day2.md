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

2、[字符串相乘](https://leetcode-cn.com/problems/multiply-strings/)
分析：模拟竖式相乘的过程，m位数与n位数相乘的位数在m+n-1 ~ m+n 之间，因此可以先预设一个 m+n 的数组存储结果，然后从尾部至头部进行进制， 最终看首位是否有非0的值（看结果是m+n位数还是m+n-1位数），将结果转换为 str 输出。 注意： 假设num1 = 560 num2 = 120，在计算的时候不可以 500 * 100 + 500 * 20 + 500 * 0 + ... ，因为题目中说了字符串可能很长，已经超过了int的表示范围，因此单拿出来各个位置上的数字进行相乘， 然后考虑进制， 是一个合理的选择。
```python3
class Solution(object):
    def multiply(self, num1: str, num2: str) -> str:
        if num1 == '0' or num2 == '0':
            return '0'
        m, n = len(num1), len(num2)
        ans = [0] * (m + n)
        for i in range(m-1, -1, -1): ## 这里第一个 -1 是让i取到0
            x = int(num1[i])
            for j in range(n-1, -1, -1):
                y = int(num2[j])
                ans[i + j + 1] += x * y

        for i in range(m + n - 1, -1, -1):
            # 从末尾开始，计算是否进1
            ans[i-1] += ans[i] // 10
            ans[i] = ans[i] % 10 

        ### 注意： 这里需要判断最高位是否非0
        index = 1 if ans[0] == 0 else 0
        ans = ''.join([ str(x) for x in ans[index:] ])
        return ans
```
