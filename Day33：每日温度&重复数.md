[每日温度](https://leetcode-cn.com/problems/daily-temperatures/)  
分析：   
> * 根据已经拿到的每日温度，然后，找到下一个温度升高的位置， 如果后面没有温度升高，则置0处理;   
> * 这道题目适合用单调栈或者单调队列来处理，之前没做过类似的题目，这个思路一开始没想到，维护一个栈底至栈顶单调递减的栈， 当遇到比栈顶温度高的时候，说明**找到了栈顶存放温度index遇见了比自己当天温度更高的时候**，这个时候更新时间差（天数），同时继续维护栈的单调性，否则就将当天的温度放入栈中，等候下一个比自己温度更高的天数；   

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        int n = T.size();
        vector<int> ans(n);
        stack<int> s; // 维护一个单调栈， 这个栈从栈底至栈顶单调递减
        // 将 温度列表的 index 放入栈中， 当遇见比当前栈顶对应的温度更高时，pop出来，并且更新 ans 对应的天数
        for(int i = 0; i < n; ++i){
            while(!s.empty() && T[i] > T[s.top()]){
                ans[s.top()] = i - s.top();
                s.pop();
            }
            s.push(i);
        }

        return ans;
    }
};
```    
---  

[重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)   
分析：   
> * 注意题目中的要求，不能修改原数组，只能使用O(1)的空间， 时间复杂度 < O(n^2)， 因此也不能暴力求解；  
> * 这个题目跟之前的只出现一次的题目还不一样，只重复一次，我们可以利用异或的方法，求得只重复一次的数字， 这里不能，因为重复数字可能不止重复一次；  
> * 大致有两个思路，一个是用cnt[i]表示数组中nums 小于等于 i 的个数， 如果这个是单调递增的， 重复数假设为 target， 那么[1, target-1]区间内，cnt[i] <= i，[target, n]区间内， cnt[i] > i， 因为有重复数字，因此可以用二分法求cnt[i]>i出现的第一个位置，思路是不停缩小[target, n]的区间（这里满足cnt > mid(  z这里 mid = (l + r)//2)）直至 l == r时，说明找到了target；  
> * 第二个思路比较难想， 借鉴查找链表中的环入口的思路，利用快慢指针相遇找到相遇点，然后再找环入口位置即使重复数字所在的地方，这里一开始的链表相当于 i --> nums[i]，就是index --> val 这样构造的链表， 因为有重复数字， 因此一定存在至少两个 index 映射 同一个 val， 这样就构成了环， 可以利用查找环入口思路去做， 这里一开始我会觉得如果存在一个一种情况: **i == nums[i]** 的情况，这样岂不是自身成环， 后来想了一下因为题目中说的是 1~n之间的数字，构成一个 n+1 大小的数组， 不存在 0 -> 0 的问题，也就是说，不可能出现链表入口就自身成环， 这样的话，比如数组 [1, 3, 2, 4, 3] 这里 2 -> 2， 但是实际上现在是**两个链表了**：1、 0 --> 1 --> 3 --> 4 --> 3 (这里3和4成环了)；2、 2 --> 2 ， 这个时候从0开始不可能会走到2链表里面，所以这种情况就正常找1链表的环入口就可以了 。   
这里基于方法2给出了实现   
```C++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0, fast = 0;
        do{
            slow = nums[slow];
            fast = nums[nums[fast]];
            // std::cout << "proc " << " slow : " << slow << " fast : " << fast << std::endl;

        } while(slow!=fast);

        slow = 0;
        while(slow!=fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};
```   

