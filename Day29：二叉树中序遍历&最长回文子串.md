[二叉树中序遍历--迭代法](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)    
分析：     
> * 这道题目是用迭代法给出二叉树中序遍历的 code    
> * 迭代法比较好用的是标记法， 将遍历的节点放入栈 stack 中，然后遍历到中间节点（根节点）时，放入标志位 -- nullptr，然后就可以根据标志位访问到放入的所有节点了     
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        std::stack<TreeNode* > q;
        std::vector<int> ans;
        if (root == nullptr)
            return ans;
        q.push(root);
        while (!q.empty()){
            TreeNode* node = q.top();
            q.pop();
            if (node != nullptr){
                if (node->right)
                    q.push(node->right);
                q.push(node);
                q.push(nullptr); // 标志位
                if (node->left)
                    q.push(node->left);
            }
            else{
                node = q.top(); // 取出当前的中间节点
                q.pop();
                ans.push_back(node->val);
            }
        }
        return ans;
    }
};
```    
---    
[最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)    
分析：    
> * 这里求最长回文子串是采取由中心向两边扩散判断是否继续是回文子串的方式进行，算法复杂度是O(n^2)，**注意：这里的中心有两种情况，1、当前某个位置 2、某连续两位置中间**    
> * 对于回文子串而言，由于回文子串的中心存在两种情况，因此我们需要分三种情况进行判断是否是回文子串   
> > 1. 首先自身本身是回文子串（i处为中心），即 s[i] == s[i]；   
> > 2. 其次连续

