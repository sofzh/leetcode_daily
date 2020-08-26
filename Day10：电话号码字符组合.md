[电话号码字符组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)     
分析：这题的思路很容易想到，就是穷举法，一个一个的拼接字符，问题在于实现，实现的时候要注意保存已有的res或者status，以便于下一步拼接字符的时候能够在原有res基础上进行拼接， 这个问题思路简单，实现比较复杂， 参考回溯法的实现过程    
```python3
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        dial_dic = {'2' : 'abc', '3' : 'def', '4' : 'ghi', \
                    '5' : 'jkl', '6' : 'mno', '7' : 'pqrs', \
                    '8' : 'tuv', '9' : 'wxyz'}
        def comb(input_str, res):
            ##  穷举法， 记录当前的 res 后面用于 添加后续的字母， 穷举法
            if input_str == '':
                return res
            # print(f'proc {input_str} , 0 : {input_str[0]} ...')
            if res == []:
                res = [str_i for str_i in dial_dic[input_str[0]]]
            else:
                len_res = len(res)
                res = res * len(dial_dic[input_str[0]])

                for idx, str_i in enumerate(dial_dic[input_str[0]]):
                    # print(f'{input_str[0]} --> {str_i} -->  {res}')
                    for ii in range(len_res):
                        res[idx * len_res + ii] += str_i
                
            return comb(input_str[1:], res)
        res = []
        return comb(digits, res)


```
