[全排列](https://leetcode-cn.com/problems/permutations-ii/)    
分析：    
&emsp; 这个题目一看就是需要用搜索回溯的方法去做，这里可以将问题看成由n个空格，我们依次向里面填充数据，但是每个数只能使用一次，很直接会想到穷举的方法，每个位置都填剩下还未使用的数字，如果填满了n个空格，则说明这个方法可行，是一个解，我们用回溯法模拟这个过程，定义递归函数backtrace(idx, perm)，当前排列为 perm，下一个待填入的位置是第 idx 个位置（下标从 0 开始）。那么整个递归函数分为两个情况：   
&emsp; &emsp; 1.如果idx==n，说明我们完成了一次排列，将perm放入ans中，这次排列递归结束；    
&emsp; &emsp; 2.如果idx<n，我们就要考虑第idx位置处填哪个数，因为不能重复，因此用vis数组记录已使用的数，如果这个数还没被标记使用，那么就尝试用这个数，对该数标记使用，进入下一位置递归backtrace(idx+1, perm)，进行否则尝试填入其他数字，但是要撤销刚才标记的数字，因此此时是不用这个数字的；    
&emsp; 但是这样做会得到答案，但是**答案中会包含很多重复的组合**，为了解重复问题，我们提前设定一个规则，保证填第idx位置，重复数字只能被填入一次，可以先对数组nums进行排序，保证重复数字都在一块，然后每次填入的数字一定是这个数字所在重复序列中**「从左至右第一个未被使用的数据」**，这样约束重复数字的使用顺序，就可以保证既能够用完重复数字也**不会出现重复数字之间的因为顺序问题而导致的重复组合**，从而达到去重的目的    
```C++
class Solution {
    std::vector<int> vis;

public:
    void backtrace(const std::vector<int>& nums, std::vector< std::vector<int>>& ans, const int idx, std::vector<int>& perm){
        if (idx == nums.size()){
            ans.emplace_back(perm);
            return;
        }
        for (int i = 0; i < nums.size(); ++i){
            if (vis[i] || (i > 0 && nums[i] == nums[i-1] && !vis[i-1])){
                continue;
                // 1 如果 i 已被选取
                // 2 如果i没被选取，但是他是重复数字，但是为了去重，我们重复数字只能选择从左至右第一个未使用的数字，如果此时i-1处的重复数字没被使用，说明这种情况后面会造成重复, 直接不再考虑即可
                
            }
            perm.emplace_back(nums[i]);
            vis[i] = 1; // 如果perm下一个位置填nums[i]
            backtrace(nums, ans, idx + 1, perm);
            vis[i] = 0; // 如果不选用这个填入
            perm.pop_back();
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        std::vector< std::vector<int>> ans;
        std::vector<int> perm;
        vis.resize(nums.size()); // vis 默认初始化0
        std::sort(nums.begin(), nums.end()); // 对nums进行排序，为了让重复的数据都在一起
        // 这样后面在perm里面放数字的时候就可以只放非重复的
        backtrace(nums, ans, 0, perm);
        return ans;
    }
};
```
