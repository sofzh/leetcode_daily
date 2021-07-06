[编辑距离](https://leetcode-cn.com/problems/edit-distance/) 
分析：   
> * 编辑距离的题目非常适合用动态规划的思路来做；  
> * 首先关于dp的定义：dp[i][j] : 定义为 以 word1[i-1] 为结尾的 str 与 以 word2[j-1] 为结尾的 str 之间的编辑距离， 这里用 i-1 和 j-1 是因为要考虑 word1 、 word2 其中有可能是空str；  
> * 转移矩阵：
> > * 当 `word1[i-1] == word2[j-1]` 时不需其他操作， `dp[i][j] = dp[i-1][j-1]`;  
> > * 当 `word1[i-1] != word2[j-1]` 时， 需要考虑 插入、删除、替换等操作， 具体如下:  
> > > i. 插入: 插入word1， word2[j-1] 保持不动， 则编辑距离为 `dp[i][j] = dp[i-1][j] + 1`， 同理插入word2， 编辑距离为 `dp[i][j] = dp[i][j-1] + 1`;  
> > > ii. 删除: **这里要注意** 删除word1 等价于 增加 word2，因此公式与 [^插入] 保持一致;  
> > > iii. 替换: 替换是在 `dp[i-1][j-1]` 基础上对 word1[i-1] or word2[j-1] 进入替换， 此时编辑距离为 `dp[i][j] = dp[i-1][j-1] + 1`;  
> * 时间复杂度和空间复杂度均为 `O(n^2)`;  
```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int> > dp(word1.size() + 1, vector<int>(word2.size() + 1, 0));
        // dp[i][j] : 定义为 以 word1[i-1] 为结尾的 str 与 以 word2[j-1] 为结尾的 str 之间的编辑距离
        // 这里为什么是 i-1 和 j-1 呢， 因为要考虑如果 word1 或者 word2 是空， 那么 dp[i][0] = i 是有意义的 ！！！
        /**
        *  转移矩阵：
        *   1. word1[i-1] == word2[j-1], --> dp[i][j] = dp[i-1][j-1]
        *   2. word1[i-1] != word2[j-1] 时， 需要考虑 三种操作 ： 插入（增加）、 删除（减少）、替换
        *       i. 插入:
        *           插入word1 --> dp[i][j] = dp[i-1][j] + 1 , 因为此时 word2 不变;
        *           插入word2 --> dp[i][j] = dp[i][j-1] + 1 ;
        *       ii. 删除:
        *           删除 word1[i-1] ==> 在 word1[i-2] 和 word2[j-2] 计算距离时， 增加 word2 , 得到 dp[i-1][j] + 1
        *           同理，删除 word2[j-1] 可类比
        *       iii. 替换:
        *           因为仅仅替换 word[i-1] or word2[j-1], 只替换一个， 因此 --> dp[i][j] = dp[i-1][j-1] + 1;
        *       综上，转移矩阵为 dp[i][j] = min(i, ii, iii)
        */  
        // 初始化: dp[i][0] = i, dp[0][j] = j , 对于其中一个空字符， 与另一个字符距离为 另一个字符长度
        for (int i = 0; i <= word1.size(); ++i) dp[i][0] = i;
        for (int j = 0; j <= word2.size(); ++j) dp[0][j] = j;
        for (int i = 1; i <= word1.size(); ++i) {
            for (int j = 1; j <= word2.size(); ++j) {
                if (word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                } else {
                    dp[i][j] = min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]} ) + 1;
                }
            }
        }
        return dp[word1.size()][word2.size()];
        
    }
};
```
