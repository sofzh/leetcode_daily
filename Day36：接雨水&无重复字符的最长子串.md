[接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)  
分析：  
> * 每个柱子上的存水量与该柱子两边的最高柱子有关系， 只考虑局部不考虑整体来看，一个柱子 i 的存水量应该是 min(l_max, r_max) - height[i]，其中l_max 和 r_max 分别是柱子左右的最高柱；  
> * 一种暴力的解法是循环遍历 i， 每次遍历柱子的时候去找他两边最长的柱子，这样很麻烦，时间复杂度是 O(n^2)，可以优化为提前找好每颗柱子对应的左侧最高柱子以及右侧最高柱子，这样时间复杂度是 O(n)，但是因为存放左最高柱和右最高柱，空间复杂度也是 O(n)；  
> * 其实柱子的存水量只和两边高柱的较矮柱有关系，可以用双指针从两边向中间夹击需要左右高柱，如果此时l_max < r_max， 那么即使对于该柱子来说他右侧的最高柱并不是现在用到的r_max 对应的柱子也无妨，毕竟只需l_max 限制住就可以了， 所以这会存水量只会和 l_max 有关系，与r_max 无关， 这时继续遍历左指针，寻找 l_max >= r_max， 这样柱子的存水量就和r_max 相关， 直到这俩指针相遇，这个思路比较巧妙；  
```C++
class Solution {
public:
// 用双指针法两边夹击， 因为各个柱子上的存水量只与他两边较短边有关系， 也就是和 min(lmax, rmax)以及自身的高度有关系
    int trap(vector<int>& height) {
        if(height.empty())
            return 0;
        int n = height.size();
        int left = 0, right = n - 1;
        int lmax = height[left], rmax = height[right];
        int res = 0; // 每个柱子上的存水量
        while(left <= right){
            lmax = std::max(lmax, height[left]);
            rmax = std::max(rmax, height[right]);
            if(lmax <= rmax){
                res += lmax - height[left];
                left++; // left 右移 找下一个 lmax
            }
            else{
                res += rmax - height[right];
                right--; // right 左移 找下一个 rmax
            }
        }
        return res;
    }
};
```  
---  
[无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)  
分析：  
> * 这里需要用 set 来记录子串，这样判断是否新加入了重复的子串；  
> * 这样可以先从一个字符的起点开始算，计算最长多少不重复子串，然后将起点的字符去掉，因为之前子串的起点至终点都是不重复的，所以起点右移一次之后，至终点也是不重复的，终点处的指针只需右移继续判断是否重复了即可； 
> * 因为题目要求求解最长不重复子串，因此当右指针至终点时构成的不重复字串，这时再右移起点不会得到比这个更长的，因此这个时候直接不需要继续遍历寻找了，只需在整个遍历过程中记录最长子串的长度即可；  
```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // 滑动窗口， 左右指针， 左指针移动 右指针也开始移动(至少从当前位置开始)
        std::unordered_set<char> occ;
        int n = s.size();
        int rk = 0, ans = 0; // 右指针初始化为 0
        for(int i = 0; i < n; ++i){

            
            while(rk < n && !occ.count(s[rk])){
                occ.emplace(s[rk]);
                rk++;
            }
            
            ans = std::max(ans, rk-1+1-i); // 这里 rk - 1才是rk位于的不重复位置 + 1 是为了统计个数  
            if(rk == n){ // 如果右指针到达终点， 不会有比这个更长的了
                return ans;
            }
            occ.erase(s[i]);
            // occ.erase(s[rk]); 

        }
        return ans;

    }
};
```

 
