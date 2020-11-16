[根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)  
分析：  
> * 首先我们需要先排序，拿到相应的相对关系，再考虑怎么重构；   
> * 因为是[h, k] 这种方式，那么我们应该怎么排序呢？ 只按照身高h排， 好像也不太对，如果大家身高都不一样，直接先按照身高排， 然后先排矮个子，因为剩下的都是比他高的， 根据k值就知道前面有几个人了（这几个人先不管到底是谁），至少矮个子的位置是前面的空格 + 1， 前面的空格对应后面要插入的高个子；  
> * 如果是相同身高呢？ **这里其实应该先插入k较大的同身高者，因为同身高k小的在k大的前面，这样k才能较大**，比如 A : [100, 2]、 B : [100, 3]，A前面有两个比A高的， B前面有3个（包含A），这里有点像是说 A 比 B 高一点点，因此先排B的话，B前面有3个空位，在把A排在这3个空位内即可，如果反过来就不好计算空位个数了；  
```C++
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        // 首先对数组进行排序， 按照身高升序， 然后相同身高按照k的降序进行排序
        // 首先先排 身高低的， 它对应的前面剩下的空位就是比这些更高的， 这样先从小的开始插入， 保证剩下的坑位都是比当前插入的数要大！！！（这时思路）
        // 如果遇见身高相同的呢？ 那么久先排 k 较大的， 因为相同身高 k 较小的排在了 大k的前面
        // 假设目前有 [100, k1] [100, k2]，k2>k1，因为[100, k1]需要在[100, k2]前面，如果先排k1，那么排k2的时候，坑位就会 < k2+1 ，而若是先排 k2，再排 k1的时候，[100, k1] 位于 [100, k2]前面的坑位中，对于k1 k2的插入均无影响
        std::sort(people.begin(), people.end(), [&](vector<int>& p1, vector<int>& p2){
            return (p1[0] < p2[0] || p1[0] == p2[0] && p1[1] > p2[1] );
        });
        const int n = people.size();
        vector<vector< int> > ans(people.size());

        int spaces;
        for(const vector<int>& p : people){
            spaces = p[1] + 1; // + 1 是因为要考虑算上自己， 应该放在第 p[1]+1 个空位上
            for(int i = 0; i < n; ++i){
                if(ans[i].empty()){
                    spaces--;
                    if(spaces == 0){
                        ans[i] = p;
                        break; // 插入
                    }
                }
            }
        }
        return ans;
    }
};
```
