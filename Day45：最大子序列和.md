[最大子序列和](https://leetcode-cn.com/problems/maximum-subarray/)  
分析：  
> * 这个题目是求**最大连续子序列和**，可以用贪心算法或者动态规划算法来做；  
> * 具体来说贪心算法就是每次子序列从正值开始，然后记录加和，如果加和一直为正，那么可能后面再加上正值会得到更大的数--即我们要求的最大和；  
> * 也可以用dp的思路来做，定义dp数组为以i为结尾的连续子序列和，那么我们要求的就是max(dp[i])，其状态转移方程是 dp[i] = max(dp[i-1] + nums[i], nums[i]) ， 以i为结尾，或者直接就是i本身；  
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if(nums.size() == 0)
            return 0;
        vector<int> dp(nums.size(), 0);
        int ans = nums[0];
        dp[0] = nums[0]; // 初始状态
        for(int i = 1; i < nums.size(); ++i){
            // 状态转移矩阵
            dp[i] = std::max(dp[i-1] + nums[i], nums[i]); // 以 i 为结尾的 最大连续区间值
            ans = std::max(ans, dp[i]);
        }
        return ans;
    }
};
```
