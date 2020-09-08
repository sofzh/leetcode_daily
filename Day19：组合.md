[组合](https://leetcode-cn.com/problems/combinations/)    
分析： 这里采用枚举的算法， 采用深度优先搜索的方式 （dfs），对于一个位置， 可以选择也可以不选择， 一共2两种情况，不过这样需要判断终止条件：    
&emsp; 1.剩下的位置不足以构成一个k长度的数组；
&emsp; 2.目前刚刚好凑齐， 则保存结果返回，下一步不选择这个位置，继续搜索符合要求的答案；  
```C++
class Solution {
public:
  //全局变量， 记录答案
    std::vector<int> tmp;
    std::vector <std::vector<int>> ans;
    void dfs(int cur, int n, int k){
        // 剪枝 tmp长度 加上 剩下的长度不可能构造成答案时，直接返回
        if (tmp.size() + (n + 1 - cur) < k)
            return;
        
        // 记录合法答案
        if (tmp.size() == k){
            ans.push_back(tmp);
            return;
        }

        // 考虑两种情况 ： 1. 选择当前位置 2. 不选择当前位置
        // 选择当前位置
        tmp.push_back(cur);
        dfs(cur+1, n, k);
        // 不选择当前位置
        tmp.pop_back();
        dfs(cur+1, n, k);

    }
    vector<vector<int>> combine(int n, int k) {
        dfs(1, n, k);
        return ans;
    }
};
```
