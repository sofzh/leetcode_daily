[二叉搜索树转化为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)    
分析：
> * 这道题目第一开始思路不是很好想，后面想到**二叉搜索树的性质**：左子树<root， 右子树>root，左子树、右子树同样也是二叉搜索树，**累加树是将比自己大的都跟自己的值相加**   
> * 刚好二叉搜索树的**中序遍历**可以得到递增序列，而我们实际需要的是一个递减，对于这个递减序列去累加数值，只需要**反向中序遍历**即可    
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int sum = 0;
    // 二叉搜索树中序遍历是递增序列，我们反中序遍历即可得到递减序列
    // 这样就可以从最高到最低，然后依次加和了
    TreeNode* convertBST(TreeNode* root) {
        if (root == nullptr)
            return root; 
        convertBST(root->right);
        sum += root->val;
        root->val = sum;
        convertBST(root->left);
        return root;
    }
};
```
