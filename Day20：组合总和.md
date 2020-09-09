[组合总和](https://leetcode-cn.com/problems/combination-sum/)    
分析：这个题是回溯算法的范例了，但是要注意回溯算法一般是需要做剪枝的，不然每个情况都遍历，太费时间，而且题目中要求答案不能重复，这里的方案是根据搜索起点，记录搜索的位置，然后一步一步遍历，这样就不会出现重复之前搜索的路径了（这个是避免重复的难点所在）    
```C++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        int len = candidates.size();
        std::vector< std::vector<int>> res;
        if (len == 0)
            return res; 
        std::vector<int> path;
        dfs(candidates, 0, len, target, path, res);
        return res;

    }

    void dfs(const std::vector<int>& candidates, const int begin, const int len, const int target, std::vector<int>& path, std::vector< std::vector<int>>& res){
        /**
        Param:
            begin : 搜索起点，为了防止搜索的路线冗余
            len : candidates 长度
            target : 目标值
            path : 路径
            res : 结果
        */
        // 如果继续搜索无法找到
        if (target < 0)
            return;
        // 已经满足target
        if (target == 0){
            res.push_back(path);
        }
        // 还需寻找，注意这里可以从搜索起点开始找，因为可以重复
        for(int i = begin; i < len; ++i){
            // 如果选择 i 位置处的值
            path.push_back(candidates[i]);
            // 注意这个起点依然是i， 因为可以重复选取， 这里容易出错
            dfs(candidates, i, len, target-candidates[i], path, res);
            // 如果不选择 i 位置处的值，则从path回退一个，在下个循环中更新i的值
            path.pop_back();

        }
        return;

    }
};
```
