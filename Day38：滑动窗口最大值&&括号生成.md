[滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)  
分析：  
> * 滑动窗口取最大，如果直接暴力模拟，遍历一个窗口求取最大值，能做是能做，但是肯定不会是这么简单直白的题目；  
> * 这里肯定是在求取一个滑动窗口中的最大值时设置了一些弯，如果能够直接在遍历的过程中就可以一直维护一个当前最大值就好了，这时就需要维护一个**单调队列**了；  
> * 如果我们构造一个单调递减的队列，**队头最大（当前最大值）**，队尾最小，（单调队列目前是没有其他库实现的），这样咱们在遍历窗口的时候，在pop掉窗口左侧值，以及push进窗口右侧值的时候对这个单调队列进行更新就好了；  
> * 问题在于怎么更新呢？pop的时候如果pop出来的数刚好就是当前的维护的最大值，那么应该在队列中也pop出，如果不是，那么说明最大值还在其他位置并不是在窗口左侧，就不需其他操作了，push的时候因为需要维护这个单调队列，因此如果push进去的数 > 队列的尾部，那么应该将小数拿出来，递归着把大数放进去，这样单调队列依然成立；  
```C++
class Solution {
private:
    class MyQueue{
        // 单调队列 从大到小(队头要大)) deque实现
        // 弹出时需要判断是否弹出了最大值来更新队列
        // pop时需要判断是否非空
    public:
        std::deque<int> que; // 实现单调队列
        void pop(int val){
            if(!que.empty() && val == que.front()){
                que.pop_front();
            }
        }
        // 将que弄成从大到小排列， 表示队头既是目前的最大值
        void push(int val){
            while(!que.empty() && val > que.back()){
                que.pop_back();
            }
            que.emplace_back(val);
        }

        int front(){
            return que.front();
        }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue que;
        vector<int> ans;
        if(k >= nums.size()){
            k = nums.size();
        }
        for(int i = 0; i < k; ++i){
            que.push(nums[i]);
            // 先放前k
        }
        ans.emplace_back(que.front()); // 当前最大
        for(int i = k; i < nums.size(); ++i){
            que.pop(nums[i-k]);
            que.push(nums[i]);
            ans.emplace_back(que.front()); // 记录新的 k 数组 中最大值
        }
        return ans;
    }
};
```  
---  
[括号生成](https://leetcode-cn.com/problems/generate-parentheses/)  
分析：  
> * 这里要生成符合规则的括号， 那么就相当于用回溯的方式给出合理的组合，因此我们采用回溯的方式去做；  
> * 
