[最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)  
分析：  
> * 这里在买卖股票期间，加上冷冻期限制；  
> * dp状态`dp[i][j]`表示第i天的状态为j所能剩的现金为`dp[i][j]`， 其中0表示持有股票， 1表示能够买入股票， 2表示处于冷冻期；  
> * 那么持有股票`dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i])`，注意**这里是前一天能买入，第二天才可以买入持有**；  
> * 能够买入`dp[i][1] = max(dp[i-1][1], dp[i-1][2])`，因为第二天就脱离冷冻期了，所以来源有两个；  
> * 冷冻期的状态只有一个`dp[i][2] = dp[i-1][2]`；  
> * 注意从递推公式来看， `dp[i][2] <= 0` 才行， 同时注意初始化状态；  
> * 时间复杂度`O(n)`， 空间复杂度`O(1)`；  
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        /*
        * 0 : 持有股票
        * 1 : 能够购买， 计算下一天持有时会用到
        * 2 : 冷冻期
        */
        int n = prices.size();
        if (n == 0) return 0;
        vector<vector<int> > dp(n, vector<int>(3, 0));
        dp[0][0] = -prices[0]; // 持有股票
        for (int i = 0; i < n; ++i) 
            dp[i][2] = -10000;
        // dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i]))
        // dp[i][1] = max(dp[i-1][1], dp[i-1][2]); 
        // dp[i][2] = dp[i-1][0] + prices[i]
        for (int i = 1; i < n; ++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i]);
            dp[i][1] = max(dp[i-1][1], dp[i-1][2]);
            dp[i][2] = dp[i-1][0] + prices[i];
            // cout << i << "  --> " << dp[i][0] << " , " << dp[i][1] << " , " << dp[i][2] << endl;
        }
        return max(dp[n-1][1], dp[n-1][2]);
    }
};
```
