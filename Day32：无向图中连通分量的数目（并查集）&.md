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
