[回文联表](https://leetcode-cn.com/problems/palindrome-linked-list/)    
分析：如果是数组判断是否是回文数组很好判断，但是链表只能按照指针一次一次向后读，就会难以判断是否是回文，因此这里需要把链表分为两部分，反转前面的链表，根后半部门链表比较看看是否是回文链表。    
具体的思路已经写在代码里了， 可以用快慢指针找链表中间位置，但是要注意如果链表长度是奇数需要在中间位置做特殊处理，具体原因已经写在注释里了.      
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        // 快慢指针找到链表中间位置，注意若链表长度奇数，需要特殊处理
        // 反转前半部门链表 和 后半部分的链表进行比较 即可判断是否是回文链表
        ListNode* fast = head; // 快指针
        ListNode* slow = head; // 慢指针
        ListNode* pre = nullptr;
        while (fast && fast->next){
            fast = fast->next->next; // 快指针一次走两步
            ListNode* tmp = slow->next; // 记录前半部指针原先的next，因为要反转所以需要提前记录
            slow->next = pre; // 反转链表指向
            pre = slow;
            slow = tmp; // 继续走下一步
        }

        if (fast){
            // 如果循环终止的时候fast不是NULL， 说明这个链表长度是奇数（因为从1开始走的，fast一次走两个位置），假设长度是2n+1，
            // 此时slow在的位置是n+1(中间位置)，fast在2n+1处（位置由1开始计算），pre在n，这时slow应该再前进一步，位于n+2，这样slow跟pre的位置刚好对称
            slow = slow->next;

        }
        while (pre){
            if (pre->val != slow->val)
                return false;
            pre = pre->next;
            slow = slow->next;
        }
        return true;

    }
};
```
