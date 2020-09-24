[二叉搜索树众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)    
分析：    
> * 因为是二叉搜索树，所以我们**用中序遍历的方式可以得到一个递增的结果序列**，这样的话，**相同的数就是相邻排列，剩下的问题就转化成如何在一个一维数组中，求取数组中的众数**    
> * 我们可以顺序扫描中序遍历序列，用 base 记录当前的数字，用 count 记录当前数字重复的次数，用 max_count 来维护已经扫描过的数当中出现最多的那个数字的出现次数，用 ans 数组记录出现的众数。每次扫描到一个新的元素，根据下面的逻辑进行判断：    
> * 首先更新 base 和 count：
>> 1. 如果当前访问元素跟 base 相等，那么 count 自加；    
>> 2. 否则，更新 base 为 当前访问的数字， 同时 count 计数重置为 1   
> * 其次更新 max_count 和 ans：    
>> 1. 如果 （count 自加之后）count == max_count ， 说明当前数字跟当前的众数出现次数一致， 将 base 加入 ans 中；    
>> 2. 如果 （count 自加之后）count > max_count，说明新的众数出现， 比之前众数出现次数都多， 这时之前的众数已经不是众数了（被超越了）， 那么重置 ans，以及将 base 放入 ans 中;   
**有个注意点：重置ans的时候vector的clear 是 O(n)的，比较慢， 这里直接重新创建一个vector，会比clear在push_back要快很多**
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

    int base, count, max_count = 0;
    // base 记录当前访问的数据， count 记录该base多少个， max_count 记录当前众数
    // ans 存储众数， 如果max_count 出现比原来的众数更多的众数， 那么clear ans
    std::vector<int> ans;

    void update(const int x){
        if (x == this->base){
            ++this->count;
        }
        else{
            // 更新count 和 base
            this->count = 1;
            this->base = x;
        }
        if (this->max_count == this->count){
            this->ans.push_back(this->base);
        }
        if (this->max_count < this->count){
            this->max_count = this->count;
            this->ans = std::vector<int> {this->base}; // 更新 新的众数
        }
    }

    vector<int> findMode(TreeNode* root) {
        if (root == nullptr)
            return this->ans;
        std::stack<TreeNode*> st;
        //std::vector<int> vals;
        st.push(root);
        while(!st.empty()){
            // 因为是二叉搜索树，因此中序遍历会得到一个递增结果序列
            TreeNode* node = st.top();
            if (node != nullptr){
                st.pop(); // 先取出 ， 后面和标记 nullptr 重新放进去
                if (node->right)
                    st.push(node->right);
                st.push(node);
                st.push(nullptr);
                if (node->left)
                    st.push(node->left);

            }
            else{
                // 遇见nullptr 开始获取 node， 输出值
                st.pop(); // nullptr
                node = st.top();
                st.pop();
                //vals.push_back(node->val);
                update(node->val);
                //std::cout << " cur is  " << node->val << "  -->   " << std::endl;
            }
        }
        // for (const auto &v : vals){
        //     update(v);
        //     std::cout << v << " --》 ";
        // }
        // std::cout << std::endl;
        return this->ans;
    }
};
```

