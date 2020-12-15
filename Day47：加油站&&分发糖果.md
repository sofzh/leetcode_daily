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
[分发糖果](https://leetcode-cn.com/problems/candy/)  
分析：  
> * 这里不能直接考虑左右， 这样会很麻烦， 很乱；  
> * 可以用两次遍历 贪心的策略进行做， 左 --> 右 遍历一次， 给出符合条件的 糖果数， 然后 右 --> 左 再遍历一次， 在第一次遍历基础上进行改变， 满足 左 --> 右 和 右 --> 左 都满足题设条件；  
> * **第二次遍历 不可以按照 左 --> 右 顺序**  比较左边和当前数值， 因为我们第二次遍历应该考虑右边的数值， 第二次遍历时应该根据右边值确定当前值， 需要从最右开始计算， 不然递推关系就是错误的；  
```C++
class Solution {
public:
    int candy(vector<int>& ratings) {
        const int len = ratings.size();
        std::vector<int>candyVec(len, 1);

        // 不能同时考虑 左右两边， 肯定会非常混乱， 这里用两次贪心 从左至右 、 从右至左 分别贪心策略 进行计算
        // 左 --> 右
        for(int i = 1; i < len; ++i){
            if(ratings[i] > ratings[i-1])
                candyVec[i] = candyVec[i-1] + 1;
            // 其他case 保持 原样
        }
        
        // 右 --> 左
        for(int i = len-2; i >= 0; --i){
            if(ratings[i] > ratings[i+1]){
                candyVec[i] = std::max(candyVec[i], candyVec[i+1] + 1);
                // 注意 这里因为之前 左 --> 右 已经 贪心过一次了， 这时 i 处的值 可能会更大 > i-1 处的值+1 ， 因此这里应该保证取更大的数， 这样保证原先 左 --> 右 遍历时的 增长趋势

            }
        }
        int ans = 0;
        for(const auto& val : candyVec)
            ans += val;
        return ans;
    }
};
```
