[股票买卖最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)  
分析：  
> * 这个题目跟之前的[股票买卖](https://github.com/sofzh/leetcode_daily/blob/master/Day14%EF%BC%9A%E5%A4%9A%E6%95%B0%E5%85%83%E7%B4%A0%26%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA.md)有点像，但是有手续费， 这里考虑用dp来做；   
> * 首先每天都会有两个状态，持有或者卖出， 而达到持有或者卖出状态只和前一天的持有或者卖出有关系，但是这里只能从前一天的可能状态进行考虑，因为我们的目标是要达到这一天的某个状态（持有、卖出），前一天的状态可能会进行更改，才能达到今天的状态， 也就是说我们没必要立马确定前一天状态到底是什么，只需要假设他的所有可能性即可，最主要的是达到今天的状态会有多少的利润（这一点想明白就很容易理解了）；  
> * 最后还可以对空间复杂度进行优化， 因为只跟上一天状态有关系， 我们只需要保存上一天的状态值即可；  
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        const int n = prices.size();
        int hold = -prices[0]; // 持有股票
        int sale = 0; // 卖出股票
        // 这里只考虑 当前天数 是持有 还是 卖出 这两种状态！！！ 这俩状态之间没有逻辑关系， 但是跟前一天的 是否卖出 和 持有 具有关系
        // 当前的卖出 可能出自 1、之前已经卖出 2、昨天没卖（这只是昨天的可能情况，昨天真正情况并不一定没卖， 这里要区分开， 今天这个状态只是 为了达到这个状态而走到这一步的）
        for(int i = 0; i < n; ++i){
            int pre_hold = hold; // 保存下前一天的持有股票钱数， 因为后面会更新这个， 因此这里需要预存
            hold = std::max(hold, sale - prices[i]); // 今天的持有 ： 1、昨天持有没卖 2、之前卖了(昨天也没持有) 今天若想持有 需要买入
            sale = std::max(sale, pre_hold + prices[i] -fee); // 今天的卖出 ： 1、之前卖了 2、昨天持有 今天卖
        }
        return sale;
    }
};
```
