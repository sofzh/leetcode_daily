[搜索旋转数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)  
分析：  
> * 首先数组本身是严格递增的，但是经过某个节点旋转之后就变成连续两段严格递增，但是不知道旋转节点在哪里，否则就是有序数组中直接用二分查找即可；  
> * 那么在这个数组怎么使用呢？其实我们可以通过判断target 和 nums[0] 和 nums[n-1] 之间的关系来判断这个数可能存在在哪个片段里；  
> * 
