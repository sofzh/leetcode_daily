[加油站](https://leetcode-cn.com/problems/gas-station/)  
分析：  
> * 从0开始计算加和， 统计最低点， 如果所有gas和 < cost和， 那么说明一圈下来不可能绕圈；  
> * rest[i] = gas[i] - cost[i] 表示经过 i 处时的 diff，那么如果 sum(diff) >= 0， 那么说明存在一个位置，从那里开始可以环绕一圈，那么我们直接找最低点的下一位；  
> * 因为这里画出来曲线表示趋势， 最低点后面的趋势整体向上增的， 不可能出现 < 0 的情况，否则就会和最低点出现悖论！！！ （反证法）；  
```C++
#include <limits.h>

class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        // 从0开始计算加和， 统计最低点， 如果所有gas和 < cost和， 那么说明一圈下来不可能绕圈
        // rest[i] = gas[i] - cost[i] 表示经过 i 处时的 diff，那么如果 sum(diff) >= 0， 那么说明存在一个位置，从那里开始可以环绕一圈，那么我们直接找最低点的下一位，
        // 因为这里画出来曲线表示趋势， 最低点后面的趋势整体向上增的， 不可能出现 < 0 的情况，否则就会和最低点出现悖论！！！ （反证法）
        int cur_sum = 0;
        int min = INT_MAX;
        int idx = -1;
        for(int i = 0; i < gas.size(); ++i){
            int rest = gas[i] - cost[i];
            cur_sum += rest;
            if(cur_sum < min){
                min = cur_sum;
                idx = i;
            }
        }
        if(cur_sum < 0)
            return -1; 
        // 此时所有的gas 之和 < 所有的 cost 之和 不可能会开一圈
        
        return (idx+1) % (gas.size()); // (idx+1) % (gas.size())  这里是循环，因为要从最低点下一位开始， 下一位有可能是循环到前面去， 但是如果循环到 0 处 那么
    }
};
```
