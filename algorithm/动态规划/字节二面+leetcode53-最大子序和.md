[53题](https://leetcode.cn/problems/maximum-subarray/description/)

## 和乘积最大子序列类似

定义的 dp 数组是: dp[i] 记录以 nums[i] 为结尾的「最大子数组和」,从而写出状态转移方程. 

dp[i] = Math.max(dp[i-1] + nums[i], nums[i])


```js
var maxSubArray = function(nums) {
  const n = nums.length

  const dp = Array(n)
  dp[0] = nums[0]

  for (let i=1; i<n; i++) {
    dp[i] =  Math.max(dp[i-1] + nums[i], nums[i])
  }
  //  求出dp 数组中的最大值 即 子序列最大和
  return Math.max(...dp)
}
```