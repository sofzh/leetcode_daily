[最少数量引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)   
分析：  
> * 这里首先要先排序， 不然不好知道哪些气球是重叠的；  
> * 用贪心策略， **当气球出现重叠的时候，只需要一根箭** ， 我们计算重叠区域的时候需要更新重叠区域的最右端，这一点容易出错；  
> * 如果和上一个重叠区域没有重叠， 那么需要用的箭数量 +1 ；  
```C++
class Solution {
public:

    inline static  bool cmp(const vector<int>& a, const vector<int>& b){
        return a[0] == b[0] ? a[1] < b[1] : a[0] < b[0];
    }

    int findMinArrowShots(vector<vector<int>>& points) {
        if(points.size() == 0)
            return 0;
        int ans = 1; // 初始化为1 至少需要一个箭射气球
        std::sort(points.begin(), points.end(), cmp);
        // 从小到大 排列气球
        for(int i = 1; i < points.size(); ++i){
            if(points[i][0] > points[i-1][1]){
                // 相邻排列的气球不挨着
                ++ans;
            }
            else{
                points[i][1] = std::min(points[i-1][1], points[i][1]);
                // 更新气球重叠区域
            }
        }
        return ans;

    }
};
```
