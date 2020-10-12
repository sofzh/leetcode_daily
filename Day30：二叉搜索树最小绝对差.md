[二叉搜索树最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)    
分析：    
> * 因为是二叉搜索树，这里直接用**中序遍历**的方式得到一个有序数组， 然后根据这个有序数组得到连续节点的差的绝对值
> * 可以用递归的方式递归中序遍历，甚至用两个节点来直接获取之前节点和当前节点，直接求取这两个节点的差的绝对值
> * 这里也可以选择用迭代的方式获取中序遍历的结果，用stack保存节点，因为用了nullptr进行标记，**这里需要注意的是在循环外pop节点的时候，if else里面就不用pop一遍了**，这个是容易写错的地方    
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
    
    int getMinimumDifference(TreeNode* root) {
        std::vector<int> list;
        std::stack<TreeNode*> st;
        if (!root){
            return 0;
        }
        auto node = root;
        st.push(node);
        // 中序遍历获取一个有序数组
        while(!st.empty()){
            node = st.top();
            st.pop(); // 删除当前节点（包括nullptr）
            if(node){
                if(node->right) st.push(node->right);
                st.push(node);
                st.push(nullptr);
                if(node->left) st.push(node->left);
            }
            else{
                // st.pop(); // del nullptr
                node = st.top();
                st.pop();
                list.push_back(node->val);
            }
        }
        int n = list.size();
        std::cout << "  list size is " << n << std::endl;
        int res = std::abs(list[0] - list[1]);
        for(int i = 1; i < n-1; ++i){
            res =  res < std::abs(list[i+1] - list[i])? res : std::abs(list[i+1] - list[i]);
        }
        return res;

    }
};
```
