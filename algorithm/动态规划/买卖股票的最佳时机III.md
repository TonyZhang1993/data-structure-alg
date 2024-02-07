给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

[题目](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/description/)

## 思路
```js
状态转移方程
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])

k = 2

所以
dp[i][2][0] = max(dp[i-1][2][0], dp[i-1][2][1] + prices[i])
dp[i][2][1] = max(dp[i-1][2][1], dp[i-1][1][0] - prices[i])


```


解答

```js
//  但当 k = 2 时，由于没有消掉 k 的影响，所以必须要对 k 进行穷举：
var maxProfit = function(prices) {
  let max_k = 2, n = prices.length

  let dp = Array.from({length: n}, () => Array.from({length: max_k+1}, () => Array(2).fill(0)))

  for (let i=0; i<n; i++) {
    //  对k 也进行遍历
    for(let k=max_k; k>=1; k--) {

      if (i-1 === -1) {
        dp[i][k][0] = 0

        dp[i][k][1] = - prices[i]
        continue
      }

      dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
      dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
    }
  }

  return dp[n-1][max_k][0]

};

关于 k 的循环解释

// 因为你注意看，dp[i][k][..] 不会依赖 dp[i][k - 1][..]，而是依赖 dp[i - 1][k - 1][..]，而 dp[i - 1][..][..]，都是已经计算出来的，所以不管你是 k = max_k, k--，还是 k = 1, k++，都是可以得出正确答案的。

// 那为什么我使用 k = max_k, k-- 的方式呢？因为这样符合语义：

// 你买股票，初始的「状态」是什么？应该是从第 0 天开始，而且还没有进行过买卖，所以最大交易次数限制 k 应该是 max_k；而随着「状态」的推移，你会进行交易，那么交易次数上限 k 应该不断减少，这样一想，k = max_k, k-- 的方式是比较合乎实际场景的。

当然，这里 k 取值范围比较小，所以也可以不用 for 循环，直接把 k = 1 和 2 的情况全部列举出来也可以

/**
 * 空间复杂度优化版本
 */
var maxProfit = function(prices) {
  // dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
  // dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])

  // dp[i][2][0] = Math.max(dp[i-1][2][0], dp[i-1][2][1] + prices[i])
  // dp[i][2][1] = Math.max(dp[i-1][2][1], dp[i-1][1][0] - prices[i])

  // dp[i][1][0] = Math.max(dp[i-1][1][0], dp[i-1][1][1] + prices[i])
  // dp[i][1][1] = Math.max(dp[i-1][1][1], - prices[i])
  //  i=-1的时候
  let dp_i10 = 0, dp_i11 = -Infinity
  let dp_i20 = 0, dp_i21 = -Infinity

  for (let price of prices) {
    dp_i20 = Math.max(dp_i20, dp_i21 + price)
    dp_i21 = Math.max(dp_i21, dp_i10 - price)

    dp_i10 = Math.max(dp_i10, dp_i11 + price)
    dp_i11 = Math.max(dp_i11, - price)
  }

  return dp_i20

};


```

其他相关题目
[买卖股票的最佳时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

也就是 k 为正无穷，但含有交易冷冻期的情况

额外条件： 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。<br>
解释：第 i 天选择 buy 的时候，要从 i-2 的状态转移，而不是 i-1 。


[力扣第 714 题「买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

也就是 k 为正无穷且考虑交易手续费的情况


[力扣第 188 题「买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/description/)

即 k 可以是题目给定的任何数的情况; 和本页的第一题类似

