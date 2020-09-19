[左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)     
分析：首先，注意题目要求求解左叶子之和，不是左子节点之和，第一开始我误以为是求解所有左子节点之和，这样是审题有误。。。（这种低级错误还是少犯，细心）    
&emsp; * 这道题目比较简单，**考察我们对于树的遍历，这个可以用前序遍历的方式遍历整棵树**，然后遍历到的节点是否是左叶子节点，如果是则做加和操作；    
&emsp; * 可以考虑加一个函数判断是否是叶节点， 然后定义一个深度优先搜索的函数，用前序遍历的方式遍历一棵树；    
&emsp; * 用递归的方式做简单一些，不过最好还是可以将循环的方式也掌握了，目前面试一般要求较高；    
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
    // 判断是否是叶节点
    bool isLeafNode(TreeNode* node) {
        return !node->left && !node->right;
    }

    // 前序遍历 遍历整棵树
    int dfs(TreeNode* node) {
        int ans = 0;
        if (node->left) {
            ans += isLeafNode(node->left) ? node->left->val : dfs(node->left);
        }
        if (node->right && !isLeafNode(node->right)) {
            ans += dfs(node->right);
        }
        return ans;
    }

    int sumOfLeftLeaves(TreeNode* root) {
        return root ? dfs(root) : 0;
    }
};

```
