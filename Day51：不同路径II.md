[不同路径II](https://leetcode-cn.com/problems/unique-paths-ii/)  
分析：  
> * 这是一个dp问题，dp[i][j]表示到达(i, j)有几种路径，初始化的时候需要dp[0][*] 、 dp[*][0] = 1，因为只有一种路径；  
> * 要注意，这里因为有障碍物，所以初始化的时候在遇见障碍物时需要置0，因为不能达那里；  
> * 递推公式为：`dp[i][j] = dp[i-1][j] + dp[i][j-1]`， 表示(i, j)处路径源自于上左俩位置；  
> * 遍历顺序为从左至右、从上至下依次遍历即可；  
```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        const int m = obstacleGrid.size();
        if(m == 0)
            return 0;
        const int n = obstacleGrid[0].size();
        vector<vector<int> > dp(m, vector<int>(n, 0));
        // 初始化 要注意初始化的时候要考虑障碍物
        for(int i = 0; i < m && obstacleGrid[i][0] == 0; ++i)
            dp[i][0] = 1;
        for(int j = 0; j < n && obstacleGrid[0][j] == 0; ++j)
            dp[0][j] = 1;
        for(int i = 1; i < m; ++i){
            for(int j = 1; j < n; ++j){
                if(obstacleGrid[i][j] == 1)
                    continue; // 保持 0 不变
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```
