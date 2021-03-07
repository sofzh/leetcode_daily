[打家劫舍](https://leetcode-cn.com/problems/house-robber/)   
分析： 
> * 这是一道dp问题的经典题目了， 不同于背包问题（0-1、完全、多重背包）， 这里是将`dp[i] 表示只能打劫前 i 家能够获取的最大收益`， 注意这里不是说必须打劫第i家， 这个是一个容易混淆的点； 
> * 状态转移方程是 `dp[i] = max(dp[i-1], dp[i-2] + nums[i])` 表示 是否考虑 打劫 第 i 家能够得到更大收益， 这个从两方面： 1、不打劫 2、打劫； 
> * 初始化的时候是 `dp[0] = 0, dp[1] = nums[0]` 这个比较容易理解，就不做解释了；  
> * 最终我们想要的结果就是 `dp[nums.size()-1]`；  
> * 时间复杂度是 `O(n)`， 空间复杂度是 `O(n)`（因为用了一个 vector），但是其实有点类似斐波那契数列， 这里可以将空间复杂度进一步优化至 只用 2 个变量保存 前一个以及前前一个， 这里没给出实现， 想要实现的可以自己实现一版；；  
```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        const int n = nums.size();
        if (n == 0)
            return 0;
        if (n == 1)
            return nums[0];
        std::vector<int> dp(n);
        // dp[i] 表示考虑 i 家可偷能够获得的最大收益， 但是要注意这里并不是代表 偷 第 i 家可以获得的最大效益， 而是考虑， 不一定偷， 这里可类比成 如果只能偷 前 i 家， 盗贼能够获取到的最大利益
        dp[0] = nums[0];
        dp[1] = std::max(nums[0], nums[1]);
        for (int i = 2; i < n; ++i) {
            dp[i] = std::max(dp[i-1], dp[i-2] + nums[i]); 
            // 1. 如果不偷 nums[i] 此时应该是 dp[i] == dp[i-1], 考虑只有 i-1 家可以偷
            // 2. 如果偷 nums[i] ， 那么此时 nums[i-1] 肯定不能偷， 不然相邻的被偷会触发报警， 因此只能 dp[i] = dp[i-2] + nums[i] 表示只考虑偷前 i-2 家能够得到的最大效益 + 当前偷 nums[i] 能够得到的最大效益之和
        }
        return dp[n-1];
    }
};
```
