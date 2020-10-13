[三数之和](https://leetcode-cn.com/problems/3sum/)    
分析:
> * 题目中要求找到所有**不重复**且和为0的三元组，这个不重复意味着我们需要对数组进行排序，不能简单的三重循环暴力求解问题，而排序之后我们就可以选择不重复的三个数字了，枚举的三元组(a,b,c)排序后找三元组就不会出现(a,c,b) 、 (b,a,c)之类的了；   
> * 直接三重循环时间复杂度很高 O(n^3)，如果找到了a + b + c = 0时， 如果此时更新b'， b' > b ， 那么可能的 c'  < c（这时a还没修改），这时我们发现当a固定时，b c 具有一定的联系，这样就可以将第三重循环变成数组从右至左的指针，这个就是**「双指针」**，双指针将复杂度由 O(n^2) 降到 O(n)    
> * 不过第一重循环跟第二重循环之间并没这个关系，因此这三重循环借助双指针，时间复杂度是 O(n^2)（排序复杂度 O(nlog(n))较小）   
     
**这里要注意循环结束的条件判断，循环结束有两种结果，详情见code**    
```C++
#include <iostream>
#include <vector>

using namespace std;

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        // 需要先排序，这样在找三元组就会避免出现重复的了
        std::sort(nums.begin(), nums.end());
        std::vector<std::vector<int> > ans;
        // 枚举a
        for(int first=0; first<n-2; ++first){
            // 保证跟上次不同
            if(first > 0 && nums[first] == nums[first-1])
                continue; 
            int third = n-1; // c 初始化为右端
            int target = -nums[first];
            // 保证 b 在 a 和 c 之间
            for(int second = first + 1; second < n; ++second){
                // 保证跟上次不同
                if(second > first+1 && nums[second] == nums[second-1])
                    continue;
                // 保证 b 在 c 左侧
                while(second < third && nums[second] + nums[third] > target){
                    --third;
                }
                // 跳出循环有两种情况， 1、指针重合--没有找到相应的值 abc， 后面 b 再自增也找不到合适的， 
                // 2、nums[second] + nums[third] <= target 要是 = 满足 则说明找到一对三元组
                // 要是 < 满足 则说明 b 需要自增，找一个更大的 b
                if(second == third)
                    break; // 跳出 for 循环
                if(nums[second] + nums[third] == target){
                    // 找到了一组三元组
                    ans.push_back({nums[first], nums[second], nums[third]});
                }
            }

        }
        return ans;
    }
};
```

---   
[两两交换链表](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)    
分析：  
> * 这个最好在纸上画一画，考虑交换时如果当前节点后面刚好交换或者当前节点后面的节点个数是奇数的时候需要怎么进行交换，刚好交换就直接交换了，如果节点个数是奇数，就将偶数个节点进行交换，最后那个节点不用管， 在交换的时候需要注意**保存当前节点信息，这个需要连接后面交换之后的节点、保存当前节点起第三个节点（可能是空节点），因为交换完需要连接上后续节点**
> * 创建虚拟头结点， 令temp表示当前节点， 初始时 temp = 虚拟头结点
> * 每次交换 temp 后面的两个节点， 要注意：**如果后面最多有1个节点，那么就不需要交换**，否则交换节点 **temp -> node1 -> node2 ---> temp -> node2 -> node1 然后更新 temp = node1**     
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* temp = dummyHead;
        // 如果 temp 后最多只有一个节点，说明不需要进行交换， 可以直接返回
        // 否则需要进行交换 temp -> node1 -> node2 ---> temp -> node2 -> node1 然后更新 temp = node1 开始对 node1 后面的节点进行交换
        while (temp->next != nullptr && temp->next->next != nullptr) {
            ListNode* node1 = temp->next;
            ListNode* node2 = temp->next->next;
            temp->next = node2;
            node1->next = node2->next;
            node2->next = node1;
            temp = node1;
        }
        return dummyHead->next; // 注意这里return 需要去掉虚拟头结点

    }
};
```
      
