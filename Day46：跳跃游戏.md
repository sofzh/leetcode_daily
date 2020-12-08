[跳跃游戏](https://leetcode-cn.com/problems/jump-game/submissions/)  
> * 一开始可能会想怎么跳跃才能跳到终点，具体是跳一步还是两步呢？ 其实后面发现这个只需要最大跳跃范围超过了终点就可以了；  
> * 因此每次更新最大跳跃范围即可， 最大跳跃范围的更新方法就是在最开始的跳跃范围(0)的范围内去判断是否更新最大跳跃范围， 如果更新了，则判断是是否满足了条件；  
```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int cover = 0;
        for(int i = 0; i <= cover; ++i){// 注意 这里是 i <= cover ： 要在跳跃范围内 更新跳跃范围 
            // 每次计算 跳跃的最大跨度， 不用管具体怎么跳， 只要这个跨度 >= 最后一位， 说明可以跳跃到
            cover = std::max(cover, i + nums[i]); // 更新跳跃范围
            if(cover >= nums.size() - 1)
                return true;
        }
        return false;
    }
};
```
