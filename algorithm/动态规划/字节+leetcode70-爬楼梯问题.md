[题目](https://leetcode.cn/problems/climbing-stairs/description/)

类似 跳台阶 问题

找规律: 

跳三级台阶等于跳两级台阶的跳法+跳一级台阶的跳法. 

跳四级台阶等于跳三级台阶的跳法+跳二级台阶的跳法. 

跳五级台阶等于跳四级台阶的跳法+跳三级台阶的跳法. 

明显也符合斐波那契数列的规律

f(n) = f(n-1) + f(n-2)
f(1) = 1 f(2) = 2 f(3) = 3 .....

```js
//  改进写法1
var climbStairs = function(n) {
  const memo = Array(n+1).fill(-1)
  const dp = (n, memo) => {
    if (n<=2) return n
    if (memo[n] != -1) {
      return memo[n]
    } else {
      memo[n] = dp(n-1, memo) + dp(n-2, memo)
    }
    // return climbStairs(n-1) + climbStairs(n-2)

    return memo[n]
  }

  return dp(n, memo)

}


//  改进写法2 - 最优
var climbStairs = function(n) {
  if (n<=2) {
    return n
  }

  let cur = 2, pre = 1, rel = 0, i = 2

  while(i++ < n) {
    rel = cur + pre
    pre = cur
    cur = rel
  }

  return rel
}
```