[无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)  
分析：  
> * 首先，看到题就知道应该是要排序，但是怎么排是个问题，按照左端点还是按照右端点进行排序呢？  
> * 直接求最少需要删减区间并不好求， 但是我们可以求它的对偶问题，求最多插入多少区间可以构成无重叠区间，这样我们就可以用贪心的思路来做，尽可能去插入更多的区间；  
> * 如果我们按照右边端点进行排序（小-->大），那么就可以从左至右遍历，因为先排小的，这样后面才有可能排更多的区间（左端点 >= **当前排列** 的右端点， 这里是当前排列，而不是上一个区间，注意！！！）；  
> * 如果我们按照左端点进行排序（大-->小），那么就需要从右至左遍历，因为右边先放大的（左端点），这样前面才可以插入更多的区间（原理同上）；  
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
