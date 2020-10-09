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

