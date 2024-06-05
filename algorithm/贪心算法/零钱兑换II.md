[题目](https://leetcode.cn/problems/coin-change-ii/description/)


给定不同面额的硬币 coins 和一个总金额 amount,写一个函数来计算可以凑成总金额的硬币组合数. 假设每一种面额的硬币有无限个. 


思路

```
有一个背包,最大容量为 amount,有一系列物品 coins,每个物品的重量为 coins[i],每个物品的数量无限. 请问有多少种方法,能够把背包恰好装满?

这个问题和我们前面讲过的两个背包问题,有一个最大的区别就是,每个物品的数量是无限的,这也就是传说中的「完全背包问题」
没啥高大上的,无非就是状态转移方程有一点变化而已. 

第一步要明确两点,「状态」和「选择」. 

状态有两个,就是「背包的容量」和「可选择的物品」,选择就是「装进背包」或者「不装进背包」嘛,背包问题的套路都是这样. 

dp[i][j] 的定义如下: 

若只使用前 i 个物品(可以重复使用),当背包容量为 j 时,有 dp[i][j] 种方法可以装满背包. 

若只使用 coins 中的前 i 个(i 从 1 开始计数)硬币的面值,若想凑出金额 j,有 dp[i][j] 种凑法. 

base case 为 dp[0][..] = 0, dp[..][0] = 1. i = 0 代表不使用任何硬币面值,这种情况下显然无法凑出任何金额; j = 0 代表需要凑出的目标金额为 0,那么什么都不做就是唯一的一种凑法. 

如果你不把这第 i 个物品装入背包,也就是说你不使用 coins[i-1] 这个面值的硬币,那么凑出面额 j 的方法数 dp[i][j] 应该等于 dp[i-1][j],继承之前的结果. 

如果你把这第 i 个物品装入了背包,也就是说你使用 coins[i-1] 这个面值的硬币,那么 dp[i][j] 应该等于 dp[i][j-coins[i-1]]. 

若只使用前 i 个物品(可以重复使用),当背包容量为 j-coins[i-1] 时,有 dp[i][j-coins[i-1]] 种方法可以装满背包. 

看到了吗,dp[i][j-coins[i-1]] 也是允许你使用第 i 个硬币的,所以说已经包含了重复使用硬币的情况,你一百个放心. 
```

```js
var change = function(amount, coins) {
  let N = coins.length
  let W = amount

  let dp = Array.from({length: N+1}, ()=> Array(W+1).fill(0))

  //  base case
  for (let i=0; i<=N; i++) {
    dp[i][0] = 1
  }

  for(let i=1; i<=N; i++) {
    for (let w=1; w<=W; w++) {
      if (w - coins[i-1] < 0) {
        dp[i][w] = dp[i-1][w]
      } else {
        //  关键！！！！ 注意dp[i]
        dp[i][w] = dp[i-1][w] + dp[i][w-nums[i-1]]

      }
    }
  }

  return dp[N][W]

}
```
锦上添花

[动态规划空间优化技巧](https://labuladong.online/algo/dynamic-programming/space-optimization/)

能够使用空间压缩技巧的动态规划都是二维 dp 问题,你看它的状态转移方程,如果计算状态 dp[i][j] 需要的都是 dp[i][j] 相邻的状态,那么就可以使用空间压缩技巧,将二维的 dp 数组转化成一维,将空间复杂度从 O(N^2) 降低到 O(N). 

```js

var change = function(amount, coins) {
    var n = coins.length;
    var dp = new Array(amount + 1).fill(0);
    dp[0] = 1; // base case
    for (var i = 0; i < n; i++) {
        for (var j = 1; j <= amount; j++) {
            if (j - coins[i] >= 0) {
                dp[j] = dp[j] + dp[j-coins[i]];
            }
        }
    }
    return dp[amount];
};

```

