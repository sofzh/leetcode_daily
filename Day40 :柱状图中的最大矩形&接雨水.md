[柱状图中的最大矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)   
分析：  
> * 维护一个单调栈（严格单调增）这样当循环访问到i的时候，栈顶 < i 高度时， 说明此时栈顶就是i对应的左端，将i push进去直至后面遇见比i更小的，将i pop出来，同时找到了右端（此时让i pop的元素）同时i pop出来时 栈顶既是 i 的左端；  
> * 如果 栈顶 >= i 高度， 那么pop栈顶直至找到比 i 小的，至于这里为什么可以取 = ， 是因为如果遇见 = 的情况，此时将 相等的 元素入栈（pop i 之后）计算这个新的元素面积时同样可以得到最大面积的， 如果是 <= 的单调栈，那么栈内相等元素的左边界将不好找到(不过后面试了一下，这样不影响找最大面积，因为如果右边相等元素算了一下后，左边的相等元面积会更大)；  
> * **这种题目，两边（一边也可以）求较大就用单调栈维护一个递减的单调栈，两边（一边）求较小用递增的单调栈，这样已放入栈中的是有序的，pop的时候pop出来的数和迫使栈进行pop的数是有大小关系的，这样就可以知道哪边更大了，用栈来比较大小或者匹配还是很方便的** ；  
```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        // 维护一个单调栈（严格单调增）这样当循环访问到i的时候，栈顶 < i 高度时， 说明此时栈顶就是i对应的左端，将i push进去直至后面遇见比i更小的，将i pop出来，同时找到了右端（此时让i pop的元素）同时i pop出来时 栈顶既是 i 的左端
        // 如果 栈顶 >= i 高度， 那么pop栈顶直至找到比 i 小的，至于这里为什么可以取 = ， 是因为如果遇见 = 的情况，此时将 相等的 元素入栈（pop i 之后）计算这个新的元素面积时同样可以得到最大面积的， 如果是 <= 的单调栈，那么栈内相等元素的左边界将不好找到(不过后面试了一下，这样不影响找最大面积，因为如果右边相等元素算了一下后，左边的相等元素更新面积会更大)
        const int n = heights.size();
        vector<int> left(n, -1), right(n, n); // 左右各放俩哨兵在 -1 和 n位置处
        stack<int> st;
        int ans = 0;
        for(int i = 0; i < n; ++i){
            while(!st.empty() && heights[st.top()] >= heights[i]){
                // 如果不单调
                right[st.top()] = i;
                st.pop();
            }
            // 如果是 空 或者 i进去可以满足单调
            if(!st.empty())
                left[i] = st.top();
            st.emplace(i);

        }
        for(int i = 0; i < n; ++i){
            // std::cout << " i  : " << i << " height is : " << heights[i] << " left side : " << left[i] << " right side is : " << right[i] << std::endl; 
            ans = std::max(ans, heights[i] * (right[i] - left[i] - 1));
            
        }
        return ans;
    }
};
```  
---  
[接雨水（单调栈求解）](https://leetcode-cn.com/problems/trapping-rain-water/)  
分析：  
