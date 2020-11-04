[搜索旋转数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)  
分析：  
> * 首先数组本身是严格递增的，但是经过某个节点旋转之后就变成连续两段严格递增，但是不知道旋转节点在哪里，否则就是有序数组中直接用二分查找即可；  
> * 那么在这个数组怎么使用呢？其实我们可以通过判断target 和 nums[0] 和 nums[n-1] 之间的关系来判断这个数可能存在在哪个片段里；  
> * 如果 nums[mid] >= nums[0] 说明[0, mid] 区间一定是单调区间， 此时如果 target在这个区间内 更新右端点， 否则更新左端点（因为这时如果只满足 target < nums[mid]有可能 target 在右半段），同理如果 nums[mid] < nums[0] 说明 [mid, n-1] 一定单调， 此时如果 target 在这个区间(target > nums[mid] && target <= nums[n-1])，那么需要更新左端点，否则更新右端点；  
> * 这个方法是不停的缩小区间进而寻找，时间复杂度是 O(log(n))；  
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = (int)nums.size();
        if (!n) {
            return -1;
        }
        if (n == 1) {
            return nums[0] == target ? 0 : -1;
        }
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) return mid;
            if (nums[0] <= nums[mid]) {
                if (nums[0] <= target && target < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[n - 1]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }
        return -1;
    }
};


```
