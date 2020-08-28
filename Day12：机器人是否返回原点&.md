[机器人返回原点](https://leetcode-cn.com/problems/robot-return-to-origin/)    
分析：这里有两个思路，因为机器人只能移动4个方向，1.模拟机器人移动，用xy坐标记录机器人的位置，如果最后机器人坐标为原点，则说明可以返回； 2.无论机器人怎么移动在x、y方向上只要分别保持左右移动次数一致、上下移动次数一致就可以保证最终回到了原点，这里可以将x、y方向分别考虑，就像计算物体的位移一样（这个思路没有写code验证），这里给出方案1的code    
```python3
class Solution:
    def judgeCircle(self, moves: str) -> bool:
        pos = [0, 0]
        if moves == '':
            return True
        for str_i in moves:
            if str_i == 'U':
                pos[1] += 1
            elif str_i == 'D':
                pos[1] -= 1
            elif str_i == 'L':
                pos[0] -= 1;
            else:
                pos[0] += 1;
        return pos == [0, 0]
```

---    
