[有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)    
分析：判断括号的有效性可以使用「栈」这一数据结构来解决，我们对给定的字符串 s 进行遍历，当我们遇到一个左括号时，我们会期望在后续的遍历中，有一个相同类型的右括号将其闭合。由于后遇到的左括号要先闭合，因此我们可以将这个左括号放入栈顶。需要注意遍历结束后需要判断栈是否清空， 有可能有一些左括号还没匹配， 这里因为如果匹配上符合要求的话，s 的长度一定是偶数，可以用这个先过滤掉一些案例。      
```python3
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) % 2 != 0:
            return False
        
        pairs = {
            ')' : '(',
            ']' : '[',
            '}' : '{'
        }

        stack = list()
        for ch in s:
            if ch not in pairs:
                ## 此时 ch 是左括号, pairs 只有右括号, stack 存左括号，当后续遍历到
                ## 对应的右括号时，弹出stack, 这里注意 list的 pop 默认从后面开始
                stack.append(ch)
            else:
                if not stack or stack[-1] != pairs[ch]:
                    ## 如果stack中无左括号 或者 stack 用于判断的括号与ch不匹配
                    return False
                ## 如果匹配 则 弹出， 继续判断stack的下一项
                stack.pop()
        ## 有可能循环结束，但是stack中还有未匹配的左括号
        if len(stack) == 0:
            return True
        return False
```
