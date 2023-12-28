[leetcode415](https://leetcode.cn/problems/add-strings/description/)

给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和并同样以字符串形式返回。

你不能使用任何內建的用于处理大整数的库（比如 BigInteger）， 也不能直接将输入的字符串转换为整数形式。
```
示例 1：
输入：num1 = "11", num2 = "123"
输出："134"

示例 2：
输入：num1 = "456", num2 = "77"
输出："533"

示例 3：
输入：num1 = "0", num2 = "0"
输出："0"
```
## 思路



## 题解
```js
//  字符串相加; result 记录结果，tmp 记录每一位计算的结果； 从最低位开始计算
var addStrings = function(num1, num2) {
  let a = num1.length, b = num2.length, result = '', tmp = 0

  while(a || b) {
    a ? tmp += +num1[--a] : ''
    b ? tmp += +num2[--b] : ''

    result = tmp % 10 + result  //  string
    if (tmp > 9) tmp = 1
    else tmp = 0
  }

  return tmp > 0 ? 1 + result : result
}
O(max(m,n))
O(1)

```