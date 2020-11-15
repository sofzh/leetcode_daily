[翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)  
分析：  
> * 首先题目要求：去除多余的空格（包括开头以及末尾空格去掉， 中间空格需要去重）， 而且只能在原来的 string 上改，这个时候用双指针最方便了；  
> * 然后是实现字符串中的单词翻转，其实这个翻转可以等价于将去除多余空格的 string 先翻转整个字符串， 然后按照单词为单位，再将单词翻转一次就可以达到目标了；  
> * 虽然想法比较容易想出来，但是实际写出的需要考虑很多细节(比如最后一个单词和空格都需要额外处理)，详情可以看代码里的注释；  
```C++
class Solution {
public:
    void reverse(string& s, int l, int r){
        // [l, r]
        // int mid = l + ((r - l) >> 1);
        for(int i = l, j = r; i < j; ++i, --j){
            std::swap(s[i], s[j]);
        }
    }

    void remove_extra_space(string& s, const int len){
        int slow = 0, fast = 0;
        
        // 去掉开头的 空格
        while(len > 0 && fast < len && s[fast] == ' '){
            fast++;
        }

        // 去掉中间多余的空格
        for(; fast < len; ++fast){
            if(slow > 0 && s[fast] == ' ' && s[fast] == s[slow-1]) // 注意 ： 这里 slow-1 是因为要比较 前一个 slow 位置是否已经有 空格， 如果已有那么现在的 fast的空格需要删除,  很容易写成 s[fast] = s[slow] 其实这个 slow 是下一个需要赋值的位置， 不是已经赋值的位置了！！！
                continue;
            s[slow++] = s[fast];
        }

        // 去掉末端 空格
        if(slow == 0){
            // 如果输入的 s 是个包含很多空格的 字符串， 此时 slow 应该是 0 ,fast 是 n
            s.resize(1);
            return;
        }
        // 如果 slow > 0
        if(s[slow-1] == ' ') // 因为之前 slow++ 造成 slow 其实多走了一步， 对应此时 s 的 size（删去开头空格 和 中间多余空格， 但是可能末尾的空格也会留下），如果末尾 slow-1 对应的是 空格， 那么 s 的size 应该 -1
            s.resize(slow-1);
        else
            s.resize(slow); // 此时末尾不是空格， 不需要额外 -1

    }

    string reverseWords(string s) {
        const int n = s.size();
        if(n == 0){
            return s;
        }
        remove_extra_space(s, n); // 去空格
        const int len = s.size(); // 去空格之后 新的 大小
        reverse(s, 0, len - 1); // 先翻转整个字符串， 自己实现的功能，如果reverse(s, 0, -1) 相当于没有 reverse

        // 开始翻转每一个单词， 空格来分别两个相邻的单词， 但是要注意 最后一个单词需要特殊处理
        int start = 0, end = 0;
        while(true){
            
            while(end < len && s[end] != ' ') // 这里需要先用 end < len 不然 s[end] 可能会越界 这里特别容易出错
                end++;
            if(end == 0){
                // 说明此时 s 是一个 空格 否则 s 将包含单词
                break;
            }
            if(end == len){
                // 最后一个单词
                reverse(s, start, end-1);
                break;
            }

            // reverse 单词
            reverse(s, start, end-1);
            start = ++end; // 注意这里也需要同时更新 end 和 start 进入下一个单词

        }
        return s;
    }
};
```
