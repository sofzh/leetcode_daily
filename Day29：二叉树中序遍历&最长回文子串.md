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
> * 对于回文子串而言，由于回文子串的中心存在两种情况，因此我们需要分三种情况进行判断是否是回文子串，其中P(i, j)表示[i, j]构成回文子串的情况:     
> > 1. 长度为1时：首先自身本身是回文子串（i处为中心， 回文子串长度为奇数），即 P(i, i) = s[i] == s[i]；   
> > 2. 长度为2时：其次连续两个位置构成回文子串（i + 0.5 处为中心，回文子串长度是偶数），即 P(i, i+1) = s[i] == s[i+1]；    
> > 3. 长度 >2 时：这时需要根据上述两种情况进行扩增，判断是否扩增之后的子串是否依然是回文子串，即 P(i, j) = P(i+1, j-1) && s[i]==s[j]，若同时满足这两个情况，则回文子串继续扩展     
```C++
class Solution {
public:
    std::pair<int, int> expand_str(const string& s, int left, int right){
        /*
        Param:
            s : string 
            left : 中心左端点
            right : 中心右端点
        Return:
            return 当前中心外扩得到回文子串的 最大 [left, right] pair 对
        */
        int n_size = s.size();
        while (left >= 0 && right < n_size && s[left] == s[right]){
            --left;
            ++right;
        }
        // 当上述循环结束时， left right 已经自减 和 自加了， 且自减自加之后不满足循环继续的条件
        // 因此这里返回值， 需回溯至上一次的满足条件的地方
        return {++left, --right};
    }
    string longestPalindrome(const string& s) {
        int start = 0, end = 0;
        int n_size = s.size();
        for (int i = 0; i < n_size; ++i){
            // 开始中心向两边扩展
            // 这里要注意 扩展中心不一定是当前位置，因为回文字符串有可能是偶数长度也有可能是奇数长度
            // 这里就存在扩展中心应该是两个字符中间区域
            auto [left1, right1] = expand_str(s, i, i); // 奇数
            auto [left2, right2] = expand_str(s, i, i+1); // 偶数
            if (right1 - left1 > end - start){
                start = left1;
                end = right1;
            }
            if (right2 - left2 > end - start){
                start = left2;
                end = right2;
            }
        }
        return s.substr(start, end - start + 1);
    }
};
```

