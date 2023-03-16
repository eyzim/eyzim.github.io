# LeetCode 121. Best Time to Buy and Sell Stock 解題紀錄



## [題目](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)


{{< admonition type=quote title="Problem">}}

You are given an array `prices` where `prices[i]` is the price of a given stock on the $i^{th}$ day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return the *maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

 

**Example 1:**
```
Input: prices = [7,1,5,3,6,4]
Output: 5
```
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**
```
Input: prices = [7,6,4,3,1]
Output: 0
```
Explanation: In this case, no transactions are done and the max profit = 0.
 

**Constraints:**

- 1 $\leq$ prices.length $\leq 10^5$
- 0 $\leq$ prices[i] $\leq 10^4$

{{< /admonition >}}


## 想法

{{< admonition type=info title="">}}


{{< /admonition >}}



{{< image src="/images/leetcode_53/exercise.png"  height="200" 
          src_s="/images/leetcode_53/exercise.png" 
          src_l="/images/leetcode_53/exercise.png" 
alt="try dynamic programming yourself" caption="try dynamic programming yourself :)" linked="false">}}

## 解法

### 解法一

```cpp
int maxProfit(vector<int>& prices) {
    
    if(prices.size()<2)    return 0;
    
    auto buy = INT_MAX, profit = 0;
    
    for(auto i:prices)
    {
        buy = min(i, buy);
        profit = max(profit, i-buy);
    }
    
    return profit;
}
```


### 解法二

```cpp
int maxProfit(vector<int>& prices) {

    int profit = 0, buy = prices[0];

    if(prices.size() < 2)   return 0;

    for(auto i=1; i<prices.size(); ++i)
    {
        if(prices[i] > prices[i-1])
        {
            profit = max(profit, prices[i] - buy);
        }
        else
        {
            buy = min(buy, prices[i]);
        }
    }
    
    return profit;
}
```

- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(n)$.



## Reference

