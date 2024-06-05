[题目](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)

## 解析

```
第一步: 定义子问题

踏上第 i 级台阶的体力消耗为到达前两个阶梯的最小体力消耗加上本层体力消耗: 

最后迈 1 步踏上第 i 级台阶: dp[i-1] + cost[i]
最后迈 2 步踏上第 i 级台阶: dp[i-2] + cost[i]

dp[i] = min(dp[i-2], dp[i-1]) + cost[i]

第三步: 识别并求解出边界条件

// 第 0 级 cost[0] 种方案 
dp[0] = cost[0]

// 第 1 级，有两种情况
// 1: 分别踏上第0级与第1级台阶，花费cost[0] + cost[1]
// 2: 直接从地面开始迈两步直接踏上第1级台阶，花费cost[1]
dp[1] = min(cost[0] + cost[1], cost[1]) = cost[1]

```

```js
let minCostClimbingStairs = function(cost) {
  //  最后一个台阶的代价为0
  cost.push(0)
  //  踏上第 i 级台阶的体力消耗
  let dp = [], n = cost.length
  dp[0] = cost[0]
  dp[1] = cost[1]

  for (let i=2; i<n; i++) {
    dp[i] = Math.min(dp[i-2], dp[i-1]) + cost[i]
  }

  return dp[n-1]
}

时间复杂度: O(n)
空间复杂度: O(n)

//  优化
let minCostClimbingStairs = function(cost) {
  //  最后一个台阶的代价为0
  cost.push(0)

  let cur = cost[1], pre = cost[0], i = 1, rel = 0

  while(i++ < cost.length-1) {
    rel = Math.min(cur, pre) + cost[i]
    pre = cur
    cur = rel
  }

  return rel

}
时间复杂度: O(n)
空间复杂度: O(1)
```