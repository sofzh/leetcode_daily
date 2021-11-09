[所有子字符串中的元音](https://leetcode-cn.com/problems/vowels-of-all-substrings/)  
解析：  
> * 这是一道动态规划的问题；  
> * 定义 `dp[i]` 为以`word[i]`为结尾字符串元音个数， 注意这个是限制以`word[i]`为结尾， 而不是到`word[i]`的所有子字符串元素；  
> * 转移方程 ：  
> > 1. 当`word[i]`为元音时， `dp[i] = dp[i-1] + i` ， 因为相对于`dp[i-1]`来说， 相当于新增`i`个元音个数， `word[0:i]、word[1:i]、...、word[i-1:i]`一共`i`个；  
> > 2. 当`word[i]`为辅音时， `dp[i] = dp[i-1]`， 此时以`word[i]`为结尾，并未有新增元音；  
> * 最后`sum(dp)`即为所有子字符串的元音个数;  
> * 时间复杂度`O(n)`，空间复杂度`O(n)`  
```C++
class Solution {
public:
    using LL = long long;
    long long countVowels(string word) {
        int n = word.size();
        vector<LL> f(n+1);
        
        unordered_set<char> st{'a', 'o','e','i','u'};
        for(int i = 1; i <= n; i++) {
            if(st.count(word[i-1])) f[i] = f[i-1] + i;
            else f[i] = f[i-1];
        }
        LL res = 0;
        for(const auto &x: f) {
            res += x;
        }
        return res;
    }
};
```  
---
