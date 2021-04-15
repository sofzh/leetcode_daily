[最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)  
分析：  
> * 一般遇到子序列基本上都是dp问题；  
> * 这里将`dp[i]`定义为以`nums[i]`为结尾的最长递增子序列的长度，注意必须以`nums[i]`为结尾，这样就固定了一个自由度， 我们只需要找`max(dp[i])`即可；  
> * 状态转移 `dp[i] = max(dp[i], dp[j] + 1)`， 这里 `j < i 而且满足 nums[j] < nums[i]`相当于在`i`前面找能够构成递增子序列中最长的那个；  
> * 对于这种自由度比较高的题目呢？ 不妨先想办法限制一下自由度， 这样我们才好找到转移关系去做题；  
> * 目前并不是最优解法， 时间复杂度是 `O(n^2)`，空间复杂度是`O(n)`；  
```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.size() <= 1) return nums.size();
        vector<int> dp(nums.size(), 1);
        // dp[i] 表示 以i为结尾的 最长严格递增子序列长度， 初始化为1 是因为自己也得算入其中
        // dp[i] = max(dp[i], dp[j] + 1); (j < i 且 nums[j] < nums[i]) 这里是找以 nums[i]结尾的最长子序列，需要找前面的看看能否构成最长递增子序列
        int ans = 0;
        for (int i = 1; i < nums.size(); ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
                ans = max(ans, dp[i]); // max
            }
        }
        return ans;
    }
};
```
