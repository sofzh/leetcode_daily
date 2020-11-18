[最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)  
分析：  
> * 用滑动窗口的方式来更新窗口；  
> * 用map来存储 s 和 t 中的字符个数， 当窗口中的 含有的 t 的字符个数 不满足时，延长窗口（右移r指针），当窗口满足时，缩小窗口（右移l指针）；  
> * 在移动的时候 l 和 r 只能同时移动一个；  
```C++
class Solution {
private:
    std::unordered_map<char, int> t_map, s_map;

    bool check(){
        for(const auto& [a, n] : t_map){
            if(s_map[a] < n)
                return false;
        }
        return true;
    }

public:
    string minWindow(string s, string t) {
        string ans = "";
        if(t.size() == 0 || s.size() == 0)
            return ans;
        for(const auto& val : t){
            t_map[val]++;
        }
        // 滑动窗口解决， 通过比较滑动窗口中的所需字符个数 来判断是否是一个区间包含了t
        int l = 0, r = 0;
        int len = s.size() + 1;
        int ans_l = -1;
        while(r < s.size()){
            // 右移 r 找到包含t的坐标
            if(t_map[s[r]] > 0){
                s_map[s[r]]++;
            }
            while(check() && l <= r){
                if( r - l + 1 < len){
                    // 如果找到小区间 进行更新
                    len = r - l + 1;
                    ans_l = l;
                }
                // 开始右移 l 缩小区间范围
                if(t_map[s[l]] > 0){
                    s_map[s[l]]--;
                }
                ++l;
            }
            ++r;
        }
        if(ans_l == -1){// 说明没找到一个符合要求的区间
            return ans;
        }
        ans = s.substr(ans_l, len);
        return ans;

    }
};
```
