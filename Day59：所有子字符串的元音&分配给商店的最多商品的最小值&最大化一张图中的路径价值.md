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
[分配给商店的最多商品的最小值](https://leetcode-cn.com/problems/minimized-maximum-of-products-distributed-to-any-store/) 
解析：  
> * 这是一道二分题目， 选取一个`mid`使得所有的商品都可以最平均分配给商店；  
> * 分配所需商店数目： `res = sum(quantity_i / mid) + sum(quantity_i % mid > 0 ? 1 : 0)`，也就是`商+余数`， 如果`res <= n` 则说明这种分配方式可以分配开商品；  
> * 因此二分法的目的就是 `res = sum([quantity_i / mid]+) <= n`，取`sum`的上限满足`<=n`中的最小的`mid`，即为**分配给商店最多商品的最小值**；  
```C++
class Solution {
public:
    int minimizedMaximum(int n, vector<int>& quantities) {
        int left = 0, right = 0;
        for (const auto &i : quantities) {
            right = right > i ? right : i;
        }
        auto check = [&](int mid) {
            if (mid == 0) return false;
            int c = 0;
            for (const auto &q : quantities) {
                c += (int)(q + mid - 1) / mid; // 向上取整
            }
            return c <= n;
        };

        while (left < right) {
            // left <= right 则不行， 可能会一直在 check(mid)中进入死循环
            int mid = left + ((right - left) >> 1); // >> 运算优先级比较低， 需要用()进行限制
            // cout << "find res in left : " << left << " and right : " << right << " , mid is " << mid << endl;
            if (check(mid)) right = mid; // 按照此时 mid 的分法， 已经可以满足分配， 但是mid不一定最小， 仍需再找
            else left = mid + 1; // 不满足分配， 需要增加 left
        }
        return left;
    }
};
```
---  
