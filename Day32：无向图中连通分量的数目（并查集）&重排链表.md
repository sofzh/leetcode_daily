[无向图中连通分量的数目](https://leetcode-cn.com/problems/number-of-connected-components-in-an-undirected-graph/)   
分析：   
> * 题目要求求出无向图中的连通分量数目，这种题目一般是使用dfs求取以及并查集，不过一般**并查集效率会更好一些**；   
> * 并查集主要参考了[深夜学算法之Union Find Set：动态连通](https://www.jianshu.com/p/b5b8d488266e)，个人认为这篇文章讲解的非常棒；   
> * 详细版可以看上面的链接，详述了并查集的基本思路，构建一个union以及一个find的过程，同时也给出了优化方案：1. 优化合并（union-by-rank） 2. 优化查找(find)，将树进行扁平化，这里的问题和这个基本上是一致的，具体实现过程可以看我的代码实现；   
```C++
class Solution {
public:
    std::vector<int> parent;
    std::vector<int> height;

    void init(int n){
        // 初始化 parent height
        parent.resize(n);
        height.resize(n, 0);
        for(int i = 0; i < n; ++i){
            parent[i] = i;
        }
    }

    // find 根
    int Find(int x){
        if(x == parent[x])
            return x;
        else{
            int res = Find(parent[x]); // 路径压缩，将树扁平化，在find迭代的时候
            // 将路经的节点都放在一个根上，然后出递归的时候将反序的节点连在这个根上
            parent[x] = res;
            return res; // 根
        }
    }

    // union 合并两个节点或者子树
    void Union(int x, int y){
        int root_x, root_y;
        root_x = Find(x);
        root_y = Find(y);
        // 不需要 union
        if(root_x == root_y)
            return;
        // 矮树放到高树下面
        if(height[root_x] > height[root_y]){
            parent[root_y] = root_x;
        }
        else if(height[root_x] < height[root_y]){
            parent[root_x] = root_y;
        }
        else{
            parent[root_y] = root_x;
            height[root_x] += 1; // 秩 ++
        }

    }

    int countComponents(int n, vector<vector<int>>& edges) {
        init(n);
        int components = 0; // 统计联通个数
        for(int i = 0; i < edges.size(); ++i){
            int a, b;
            a = edges[i][0];
            b = edges[i][1];

            Union(a, b);

        }
        for(int i = 0; i< n; ++i){
            if(parent[i] == i){
                components++; // 找到根处
            }
        }
        return components;
    }
};
```
---  
[重排链表](https://leetcode-cn.com/problems/reorder-list/)  
分析：  
> * 这里是将l0->l1->l2->...->l_n-1->l_n 变成 l0->l_n->l1->l_n1->...，差不多就是将l0->l1->...->l_middle 和 l_n->l_n-1->...->l_middle+1这两个链表进行合并，这里关键的地方在于想到这里合并这俩链表;   
> * 还有一种方式是因为链表不好直接下标访问，可以用线性表存储链表，用线性表直接重建链表，这个方法时间复杂度是O(N)，空间复杂度也是O(N)，而直接拆分链表+翻转+合并的时间复杂度是O(N)，空间复杂度是O(1)，比之前那个方法要好，因此这里实现了这种方式;  
> *  快慢指针找到链表中间   
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

void show_list(ListNode* head){
    if(!head)
        return;
    ListNode* l = head;
    while(l){
        std::cout << l->val << " ";
        l = l->next;
    }
    std::cout << std::endl;
    return;
}

class Solution {
public:
    void reorderList(ListNode* head) {
        // 1.快慢指针找到中间节点
        // 2.将右边链表进行翻转
        // 3.合并两个链表
        if(!head)
            return;
        // std::cout << " head is " << std::endl;
        // show_list(head);
        ListNode* mid = midNode(head);
        // std::cout << " mid node is " << mid->val << std::endl;
        ListNode* l1 = head;
        ListNode* l2 = mid->next;
        // std::cout << " l1 is " << std::endl;
        // show_list(l1);
        // std::cout << " l2 is " << std::endl;
        // show_list(l2);

        mid->next = nullptr;
        // std::cout << " after reverse " << std::endl;
        l2 = reverseNode(l2);
        // std::cout << " l1 is " << std::endl;
        // show_list(l1);
        // std::cout << " l2 is " << std::endl;
        // show_list(l2);

        mergeNode(l1, l2);
        // std::cout << " after merge " << std::endl;
        // show_list(head);
        return;
    }

    ListNode* midNode(ListNode* head){
        ListNode* slow = head;
        ListNode* fast = head;
        while(fast->next && fast->next->next){
            slow = slow->next; 
            fast = fast->next->next;
        }
        return slow; 
    }

    ListNode* reverseNode(ListNode* head){
        ListNode* pre = nullptr;
        ListNode* cur = head; 
        while(cur){
            ListNode* node_tmp = cur->next; // 获取之前的next
            cur->next = pre; // 翻转链表
            pre = cur; // 更新pre
            cur = node_tmp; // 更新cur
        }
        return pre;
    }

    void mergeNode(ListNode* l1, ListNode* l2){
        // l1 = merge(l1, l2)
        ListNode* l1_tmp;
        ListNode* l2_tmp;
        while(l1 && l2){
            l1_tmp = l1->next; // 这里可能是 nullptr
            l2_tmp = l2->next;

            l1->next = l2;
            l2->next = l1_tmp; 

            // 更新 l1 l2
            l1 = l1_tmp;
            l2 = l2_tmp;
        }
    }
};
```
