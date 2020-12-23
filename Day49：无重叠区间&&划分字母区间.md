[无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)  
分析：  
> * 首先，看到题就知道应该是要排序，但是怎么排是个问题，按照左端点还是按照右端点进行排序呢？  
> * 直接求最少需要删减区间并不好求， 但是我们可以求它的**对偶问题，求最多插入多少区间可以构成无重叠区间**，这样我们就可以用贪心的思路来做，尽可能去插入更多的区间；  
> * 如果我们按照右边端点进行排序（小-->大），那么就可以从左至右遍历，因为先排小的，这样后面才有可能排更多的区间（左端点 >= **当前排列** 的右端点， 这里是当前排列，而不是上一个区间，注意！！！）；  
> * 如果我们按照左端点进行排序（大-->小），那么就需要从右至左遍历，因为右边先放大的（左端点），这样前面才可以插入更多的区间（原理同上）；  
> * 时间复杂度是 O(nlog(n) + O(n)) = O(nlog(n))，空间复杂度是O(1)；  
```C++
class Solution {
public: 
    static bool cmp(const vector<int>& a, const vector<int>& b){
        return a[1] < b[1];
    }

    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int ans = 0;
        const int len = intervals.size();
        if(len == 0)
            return ans;
        std::sort(intervals.begin(), intervals.end(), cmp);
        int right_side = intervals[0][1];
        ++ans; // ans = 1 注意这里因为先放入了一个， 所以 ans 从 1 开始计算
        for(int i = 1; i < len; ++i){
            if(right_side > intervals[i][0])
                continue;
            ++ans;
            right_side = intervals[i][1];
        }
        return len - ans;

    }
};
```  
---  
[划分字母区间](https://leetcode-cn.com/problems/partition-labels/)  
分析：  
> * 一开始可能想到用回溯， 其实这里不需要用回溯的思路，而是去找字母的最右边界，如果一段字母区间中的所有字母在后面片段的字母中都没有包含相同的字母，并且这个片段中所有字母最右位置都在这个片段中， 那么这个片段便可以划分出来，并且这样划分能够尽可能的划分出最多的区间， 这是一种贪心的思路 -- 找两个相同字母划分更多的区间；  
> * 因为是小写字母，这里我们就需要一个 map ， 来记录26个小写字母在字符串中的最右边界， 然后遍历整个字符串，如果 **当前字符串片段开头对应的最有边界是整个片段区间的最右， 那么这个片段就是需要划分出来的最小长度片段**， 后面继续按照这个思路；  
> * 问题在于： 怎么判断我们到达了这个片段区间的最右端了呢？其实就是当遍历到目前片段区间的max(区间字母最右位置)的时候，即说明目前这段字母区间所有的字母最右均 <= 这个区间的最右， **并且可以取到“=”**；  
> * 时间复杂度是O(n)， 空间复杂度是O(26)，因为有26个小写英文字母；  
```C++
class Solution {
public:
    vector<int> partitionLabels(string S) {
        int hash_map[26] = {0}; // 记录 S 中字符出现的最后位置 
        vector<int> ans;
        const int len = S.size();
        if(len == 0)
            return ans;
        for(int i = 0; i < len; ++i){
            hash_map[S[i] - 'a'] = i;
        }
        
        int l = 0, r = 0;
        for(int i = 0; i < len; ++i){
            r = std::max(r, hash_map[S[i] - 'a']); // 找到字符出现的最远位置
            if(i == r){
                ans.emplace_back(r - l + 1);
                l = r + 1; // 从上个片段的下一位开始
            }
        }
        return ans;

    }
};
```
