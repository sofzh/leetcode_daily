[数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)   
分析：    
&emsp; 因为给的数组是由范围限制的[1, n] ，要找到在这个范围内出现的数字     
&emsp; 1. 使用hash表记录，看看哪些数字没有出现；
&emsp; 2. 考虑不使用额外空间，因为数组的范围是限制的，因此可以取巧，让nums数组中对应的idx处的数改为负数， 这样就可以根据nums中哪处idx的num值是正的，就可以知道哪个idx不存在了    
```C++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int len = nums.size();
        for (int i=0; i < len; ++i){
            // 由于不使用额外空间，因此这里选择不使用hash表记录已经访问的数据
            int idx = std::abs(nums[i]) - 1;
            nums[idx] = -std::abs(nums[idx]); // 这里将nums[i]对应的idx处的数据改为负数，这样表示已经有这个数据了, 这样最后访问的时候，如果位置i处的num是正的，就说明不存在i+1

        }
        std::vector<int> res;
        
        for (int i=0; i<len; ++i){
            if (nums[i] > 0){
                res.push_back(i + 1);
            }
        }
        return res;
    }
};
```
