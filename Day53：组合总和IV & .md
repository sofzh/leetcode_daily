[组合总和IV](https://leetcode-cn.com/problems/combination-sum-iv/)   
分析：  
> * 这里看给的示例应该是求全排列，因为不限制次数，这里可以看做是一个全背包问题；  
> * 递推公式: `dp[i] += dp[i - nums[j]]`， 这里 `dp[i]表示能够装满容量为i的背包的排列个数， j 表示第j个物件--nums[j]`；  
> * 如果是 **组合那么应该先遍历物体再遍历背包（此时物体已经先排序，所以能放入背包并满足最大容量只有一个组合），这里是排列，应该先遍历背包在遍历物件， 这样就会得到排列数（放入背包的顺序不同就会带来不同的组合）** ； 
> * 时间复杂度是 `O(M * N)，M是背包容量， N 是物体个数`， 空间复杂度是 `O(M)` ；  
```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        // 全背包
        // 这里要求全排列， 所以遍历的时候需要先 遍历背包 再遍历物体
        // 如果是组合 遍历顺序为 外层遍历物体， 内层遍历背包
        vector<long long> dp(target+1, 0); // 测试数据中含有 超过int 范围的 这里尝试用 long
        dp[0] = 1;
        for(int i = 0; i <= target; ++i){
            // 遍历背包容量
            for(int j = 0; j < nums.size(); ++j){
                if(i - nums[j] >= 0 && dp[i] < INT_MAX - dp[i - nums[j]]){
                    dp[i] += dp[i - nums[j]]; // 更新 放入 j 物体， 一共有多少种方法
                }
            }
        } 
        return dp[target];
    }
};
```
