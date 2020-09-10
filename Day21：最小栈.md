[最小栈](https://leetcode-cn.com/problems/min-stack/)   
分析：这里采用空间换时间大法，维护一个栈的同时，维护一个辅助栈，这个辅助栈用于记录，当前栈中的最小值，与栈保持同步更新    
&emsp; 1. 栈放入数据时，辅助栈也放入数据，辅助栈用来记录加入这个数据后，目前的最小值是什么，更新最小值放入辅助栈    
&emsp; 2. 栈取出数据时，要让辅助栈与栈保持同步，同步pop    
```C++
#include <stack>

class MinStack {
public:
    /** initialize your data structure here. */
    std::stack<int> x_stack; // 实际用栈
    std::stack<int> min_stack; // 辅助栈，用来存储当前状态下的最小值， 与x_stack保持同步

    MinStack() {
        min_stack.push(INT_MAX);
        // std::cout<< "int max is "<< INT_MAX << " int min is " << INT_MIN \
        //     << std::endl;
    }
    
    void push(int x) {
        x_stack.push(x);
        min_stack.push(min(min_stack.top(), x)); // 更新最小值
    }
    
    void pop() {
        x_stack.pop();
        min_stack.pop();
    }
    
    int top() {
        return x_stack.top();
    }
    
    int getMin() {
        return min_stack.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```
---    
记录一下看到的一个不使用额外空间的算法[来源](https://leetcode-cn.com/problems/min-stack/solution/zui-xiao-zhan-by-leetcode-solution/)，不过我觉得细节方面还可以继续完善一些    
```python3
class MinStack:
    def __init__(self):
        """
        initialize your data structure here.
        """
        self.deq = deque()
        self.min_value = -1

    def push(self, x: int) -> None:
        if not self.deq:
            self.deq.append(0)
            self.min_value = x
        else:
            diff = x-self.min_value
            self.deq.append(diff)
            self.min_value = self.min_value if diff > 0 else x

    def pop(self) -> None:
        if self.deq:
            diff = self.deq.pop()
            if diff < 0:
                top = self.min_value - diff
                self.min_value = top
            else:
                top = self.min_value + diff
            return top

    def top(self) -> int:
        return self.min_value if self.deq[-1] < 0 else self.deq[-1] + self.min_value

    def getMin(self) -> int:
        return self.min_value if self.deq else -1
```
