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
> * 我们需要考虑回溯的终止条件：当前path中放入了n个括号组合，单层逻辑：最终形成合理的括号组合，应该是左括号的个数 == 右括号的个数，同时左括号应该在左边，如果某个位置处[0, i]区间内，左括号个数 <= 右括号个数， 那么这个组合一定不合理， 而且左括号一定要放前面；  
> * 如果每个位置处，左括号的个数 >= 右括号个数， 那么说明当前这个状态是合理状态，直至所有括号都添加进去，左右括号个数一致，因此剪枝过程是这样的：如果当前位置处，右括号个数 <= 左括号个数，那么久可以添加右括号或者添加左括号；  
```C++
class Solution {
public:
    void backtrack(vector<string>& ans, string& cur, const int left, const int right, const int n){
        if(cur.size() == 2*n){
            ans.emplace_back(cur);
            return;
        }
        if(left < n){
            // 如果这时可以添加 (
            cur.push_back('(');
            backtrack(ans, cur, left+1, right, n);
            cur.pop_back();
        }

        // 如果可以添加 ')' 这时右括号的个数 一定 < 左括号个数
        if(right < left){
            cur.push_back(')');
            backtrack(ans, cur, left, right+1, n);
            cur.pop_back();
        }
    }

    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        string s;
        backtrack(ans, s, 0, 0, n);
        return ans;
    }
};
```
