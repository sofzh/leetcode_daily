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
> * 这里需要用 set
