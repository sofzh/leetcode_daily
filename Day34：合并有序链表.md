[合并有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)   
分析：   
> * 这道题目是合并两个有序（升序）链表，得到一个升序链表；  
> * 思路比较简单，直接用迭代的方式直接实现了，要注意在实现的时候，**不能出现对 nullptr进行赋值的情况（这样是错误的）**，因此可以先定义一个无用的头结点，然后依次执行合并，当一个链表合并结束的时候，再将剩下的链表直接合并过去即可；  
> * 当然这个也有递归的写法；   
**迭代解法**   
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* ans = new ListNode(-1);
        ListNode* node = ans;
        while(l1 && l2){
            if(l1->val <= l2->val){
                node->next = l1;
                // std::cout << " proc l1 : " << l1->val << std::endl;
                l1 = l1->next;
                
            }
            else{
                node->next = l2;
                // std::cout << " proc l2 : " << l2->val << std::endl;
                l2 = l2->next;
            }
            // std::cout << " " << node->next->val << "  --> " ;
            node = node->next;
        }
        if(!l1){
            node->next = l2;
            // std::cout << " append l2: " << l2->val << std::endl;
        }
        else{
            node->next = l1;
            // std::cout << " append l1: " << l1->val << std::endl;
        }

        return ans->next;
    }
};
```   
**递归解法**   
```C++
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        } else if (l2 == null) {
            return l1;
        } else if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }

    }
}


```   
这两种

