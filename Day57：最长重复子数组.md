[最长重复子数组](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)  
> * 其实这里是最长连续子序列，这种情况一般比较适用于动态规划来做；  
> * 确认dp数组含义： `dp[i][j] 表示 以 i 为结尾的 A 数组 与 以 j 为结尾的 B 数组的最长连续子序列的长度`；  
> * 递推公式： 需要考虑两种情况， 1、 `i, j 均 > 0， 状态可以从之前状态进行推导， 此时 dp[i][j] = dp[i-1][j-1] + 1 if A[i] == B[j] else 0` ，2、`i == 0 or j == 0 ， 这时不能由之前状态推导， 只能初始化为1（代表刚找到的意思）`;  
> * 初始化为0即可， 因为表示刚开始都没找到相同的地方， 遍历顺序 先遍历A或者B 都可以， 因为定义`dp`的时候是把A放前面，这里优先遍历A，比较符合思维习惯；  
> * 时间复杂度 `O(m*n)`，空间复杂度`O(m*n)` ， 当然这里还可以用 **“滚动数组”** 的方式进行空间优化，这里可以后面再优化;  
下面是无空间优化方法：  
```C++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int> > dp(nums1.size(), vector<int>(nums2.size(), 0));
        // dp[i][j] : 以 i 为结尾的 A 和 以 j 为结尾的 B 的最大公共子数组
        int ans = 0;
        for (int i = 0; i < nums1.size(); ++i) {
            for (int j = 0; j < nums2.size(); ++j) {
                if (nums1[i] == nums2[j]) {
                    if (i > 0 && j > 0) {
                        dp[i][j] = dp[i-1][j-1] + 1;
                    } else {
                        dp[i][j] = 1; // 初始化， 找到了刚开始匹配的case
                    }
                    ans = max(ans, dp[i][j]);
                }
            }
        }
        return ans;
    }
};
```  
空间优化思路（需要说明：这里dp定义的 i-1、j-1位置和上面 i、j位置不一样，所以写法不一样）：  
```C++
class Solution {
public:
    int findLength(vector<int>& A, vector<int>& B) {
        vector<int> dp(vector<int>(B.size() + 1, 0));
        int result = 0;
        for (int i = 1; i <= A.size(); i++) {
            for (int j = B.size(); j > 0; j--) {
                if (A[i - 1] == B[j - 1]) {
                    dp[j] = dp[j - 1] + 1;
                } else dp[j] = 0; // 注意这里不相等的时候要有赋0的操作 
                if (dp[j] > result) result = dp[j];
            }
        }
        return result;
    }
};
```
