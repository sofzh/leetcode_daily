[子集](https://leetcode-cn.com/problems/subsets/)    
分析：
> * 这里数组假设有n位，那么可以根据第 i 位放不放进去来构造可能的子集；    
> * 这里一共是有2^n种放法，因此可以直接遍历出这些中放法，直接求出子集；  
> * 用mask的二进制来决定到底将第几个数放入子集中；    
> * 这里详细的解释借鉴了[官方解法](https://leetcode-cn.com/problems/subsets/solution/zi-ji-by-leetcode-solution/)    
```C++
class Solution {
public:
    std::vector<int> t;
    std::vector< std::vector<int>> ans;

    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        // 遍历mask可能的取值范围 2^n
        for (int mask = 0; mask < (1<<n); ++mask){
            // 1 << n 等价于 2**n （n>=0）
            t.clear();
            for (int i = 0; i < n; ++i){
                // i 表示 第i 位， 若i是1 则说明这位数是需要放入t的，如果不是则不放入t
                if (mask & (1<<i)){
                    // 判断mask的第i位是否是1, 因为目前 1<<i 只有第i位是1，其他是0
                    t.push_back(nums[i]);
                }
            }
            ans.push_back(t);

        }
        return ans;
    }
};
```
