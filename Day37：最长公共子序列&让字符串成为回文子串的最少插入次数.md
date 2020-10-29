[最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)  
分析：  
> * 最长公共子序列(LCS)，注意子序列不是子串，可以是不连续的，但是顺序要跟之前保持一致的;  
> * 对于公共子序列问题都是用两个指针分别在两个字符串上移动， 其暴力解法复杂度相当高，因此这种类型的题目较为适合用动态规划的做法来做；  
> * 这里采用自底向上的方式来求解，定义dp[i][j]表示 text1[0, ... , i-1] 和 text2[0, ... , j-1]的LCS长度，其中dp[0][...] = 0 和 dp[...][0] = 0，表示其中有一个空字符串，因此dp是一个(n1+1)x(n2+1)的矩阵，n1 n2是 text1 text2长度；  
> * base case 已经找到了， 那么剩下就是找状态转移方程了，dp[i][j]与dp[i-1][j-1]、dp[i-1][j]、dp[i][j-1]之间有什么关系呢？一步一步考虑，如果之前的dp已经算出来了，那么dp[i][j]表示text1的i和text2的j之间的关系，如果text1[i] == text2[j]说明这时i j 应该在lcs中 那么dp[i][j] = dp[i-1][j-1] + 1，否则i或者j 都有一定可能在LCS中，所以 dp[i][j] = max(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])，因为第三项一定不会比前两项大，因此简化为 dp[i][j] = max(dp[i-1][j], dp[i][j-1])；   
```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        if(n1 == 0 || n2 == 0)
            return 0;
        vector<vector<int> > dp(n1+1, vector<int>(n2+1, 0)); // 因为 dp[0][:] 和 dp[:][0] 意味着是其中有一个是空字符串， lcs 是 0
        // dp[i][j] 表示 text1[0,...,i-1] 和 text2[0,...,j-1]的LCS长度， dp[0]是空
        for(int i = 1; i <= n1; ++i){
            for(int j = 1; j <= n2; ++j){
                if(text1[i-1] == text2[j-1]){
                    dp[i][j] = dp[i-1][j-1] + 1; // 如果匹配上了， i j 在 lcs 中

                }
                else{
                    // 如果没匹配上 说明 i j 位置处的字符 至少有一个不再 lcs 中
                    dp[i][j] = std::max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        return dp[n1][n2];
    }
};
```  
---  
[字符串变成回文串的最小插入次数](https://leetcode-cn.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)   
分析：  
**这道题比较难**  
> * 一般没啥明显思路的，不妨想一下是否用动态规划的思路去求解呢；  
> * 如果我们定义dp[i][j]表示s[i,j]至少插入变成回文串的次数，那么base case ： dp[i][i]=0 本身就是回文串；  
> * 状态转移方程： dp[i][j] 与 之前的关系是： 1. 如果s[i] == s[j] 那么  dp[i][j] == dp[i+1][j-1] 2. 如果不等，那么 dp[i][j] = min(dp[i][j-1] + 1, dp[i+1][j] + 1)这两种情况； 
```C++
class Solution {
public:
    int minInsertions(string s) {
        // 参考 ： https://mp.weixin.qq.com/s/C14WNUpPeBMVSMqh28JdfA
        int n = s.length();
        vector<vector<int> > dp(n, vector<int>(n, 0)); // dp_table
        // dp[i][j] 表示 s[i, j]的字符串需要至少插入多少次才可以变成回文子串
        // 目标 dp[0][n-1]
        // dp[i][i] = 0
        // dp[i][j] = 1. dp[i+1][j-1] if s[i]==s[j] 2. min(dp[i+1][j], dp[i][j-1]) + 1 if s[i]!=s[j] 不能直接 dp[i+1][j-1] + 2 因为 [i+1, j]和[i, j-1]有可能本身就是回文子串，这时只要选择这俩最小的一个去+1即可，如果这俩都不是回文子串，那么这个结果跟[i+1,j-1]+2 得到的结果是一样的

        // 从下向上遍历
        for(int i = n-2; i >= 0; --i){
            // 从左向右遍历
            for(int j = i+1; j < n; ++j){
                if(s[i] == s[j])
                    dp[i][j] = dp[i+1][j-1];
                else{
                    dp[i][j] = std::min(dp[i+1][j], dp[i][j-1]) + 1;
                }
            }
        }

        return dp[0][n-1];
    }
};
```
