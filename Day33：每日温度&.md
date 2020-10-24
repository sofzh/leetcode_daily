[每日温度](https://leetcode-cn.com/problems/daily-temperatures/)  
分析：   
> * 根据已经拿到的每日温度，然后，找到下一个温度升高的位置， 如果后面没有温度升高，则置0处理;   
> * 这道题目适合用单调栈或者单调队列来处理，之前没做过类似的题目，这个思路一开始没想到，维护一个栈底至栈顶单调递减的栈， 当遇到比栈顶温度高的时候，说明**找到了栈顶存放温度index遇见了比自己当天温度更高的时候**，这个时候更新时间差（天数），同时继续维护栈的单调性，否则就将当天的温度放入栈中，等候下一个比自己温度更高的天数；   

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        int n = T.size();
        vector<int> ans(n);
        stack<int> s; // 维护一个单调栈， 这个栈从栈底至栈顶单调递减
        // 将 温度列表的 index 放入栈中， 当遇见比当前栈顶对应的温度更高时，pop出来，并且更新 ans 对应的天数
        for(int i = 0; i < n; ++i){
            while(!s.empty() && T[i] > T[s.top()]){
                ans[s.top()] = i - s.top();
                s.pop();
            }
            s.push(i);
        }

        return ans;
    }
};
```    
---  

