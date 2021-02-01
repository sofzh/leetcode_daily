[分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)  
分析：  
> * 一开始想到回溯方法，但是超时了， 这里这个问题的关键在于能否想到用 *0-1 背包* 的思维来考虑堆放问题；  
> * 因为这个相当于给一个容量为 `target == sum(nums)//2` 的背包，看最大能放入物体总和是否 等于 `sum(nums) // 2`；  
> * 这里隐藏了一个关键： *因为背包所能装下的东西重量和 <= 背包容量*， 这里如果最终 `dp[target] == target`， 说明可以找到一种方式实现等分子集；
> * 所以这里的状态转移公式依然是 `dp[i] = max(dp[i], dp[i-nums[j]] + nums[j])`，我们就是要看它得到的最大值是否可以满足最终相等的要求；  
> * 时间复杂度 `O(n^2)` ， 空间复杂度 `O(n)`；  
```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        /* dp[i] 表示容量为 i 的子集（ 背包）能够存放下的最大元素和
        * dp[i] == i 说明 容量 i 的子集 说明子集中的 和 刚好可以填满容量
        * 输入的元素和 <= 200 * 100 = 20000
        */
        
        for(const auto& i : nums){
            sum += i;
        }
        if(sum % 2 == 1){
            // 奇数 不可能把子集分成两半
            return false;
        }
        int target = sum / 2;
        std::vector<int> dp(target+1, 0);
        // 用 0-1 背包的思路来做，如果 dp[target] == target 说明找到了可以将子集分为两半的方法
        // 这里有一个潜在的条件 : 背包的最大 价值  <= 背包本身的 容量， 也就是说 dp[i] <= i ， 如果存在
        // 一张分割方式， 使得可以对半分割子集， 那么 dp[target] == target 这里必须相等才可以
        for(int i = 0; i < nums.size(); ++i){
            // 开始遍历 物体 / 子集元素
            for(int j = target; j >= nums[i]; --j){
                // 反向遍历， 防止重复放入同一件 物体/子集元素
                dp[j] = std::max(dp[j], dp[j - nums[i]] + nums[i]);
            }
        }
        return dp[target] == target;
    }
};
```   
---  
[最后一块石头重量II](https://leetcode-cn.com/problems/last-stone-weight-ii/submissions/)  
分析：  
> * 这里其实就是把石头尽量分成均等重量；  
> * 尽可能让两边石头重量相近，这样得到的才是最小石头重量， 跟上面分子集问题极其相似；  
