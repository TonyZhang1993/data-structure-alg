[leetcode8](https://leetcode.cn/problems/string-to-integer-atoi/description/)

请你来实现一个 myAtoi(string s) 函数，使其能将字符串转换成一个 32 位有符号整数(类似 C/C++ 中的 atoi 函数）. 

函数 myAtoi(string s) 的算法如下: 

- 读入字符串并丢弃无用的前导空格
- 检查下一个字符(假设还未到字符末尾）为正还是负号，读取该字符(如果有）.  确定最终结果是负数还是正数.  如果两者都不存在，则假定结果为正. 
- 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾. 字符串的其余部分将被忽略. 
- 将前面步骤读入的这些数字转换为整数(即，"123" -> 123， "0032" -> 32）. 如果没有读入数字，则整数为 0 . 必要时更改符号(从步骤 2 开始）. 
- 如果整数数超过 32 位有符号整数范围 [−2^31,  2^31 − 1] ，需要截断这个整数，使其保持在这个范围内. 具体来说，小于 −2^31 的整数应该被固定为 −2^31 ，大于 2^31 − 1 的整数应该被固定为 2^31 − 1 . 
- 返回整数作为最终结果. 
注意: 

本题中的空白字符只包括空格字符 ' ' . 
除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符. 
## 思路
忽略开头空格
忽略整数部分后的字符
返回有符号整数
范围在 32 位内
函数不能进行有效的转换时，请返回 0

所有的条件 parseInt 都满足，除了:
- 范围在 32 位内(含）
- 函数不能进行有效的转换: parseInt 返回的是 NaN

## 题解
```js
//  parseInt; 
var myAtoi = function(s) {
  // parseInt
  const numbers = parseInt(s)

  if (isNaN(numbers)) {
    return 0
  } else if (numbers < Math.pow(-2, 31) || numbers > Math.pow(2, 31) - 1){
    return numbers < Math.pow(-2, 31) ? Math.pow(-2, 31) : Math.pow(2, 31) - 1
  } else {
    return numbers
  }

}

//  非parseInt
var myAtoi = function(s) {
  let res = 0
  //  移除多余的空格
  s = s.trimStart()
  //  这种匹配方式，适用任何字符串，肯定可以匹配出结果
  match = s.match(/[+|-]?\d*/g)[0]
  if (match === '-' || match === '+' || match === '') {
    return 0
  }

  res = +match
  if (res > Math.pow(2, 31) - 1) return Math.pow(2, 31) - 1
  if (res < Math.pow(-2, 31)) return Math.pow(-2, 31)
  //  2 ** 31

  return res
}


//  tip:
let str="Mr Blue has a blue house and a blue car";
str.match(/blue/gi) //  ['Blue', 'blue', 'blue']
//  返回匹配的字符串

str.search(/bcd/)   // 输出结果: 1
//  返回值:  返回 str 中第一个与 regexp 相匹配的子串的起始位置. 
```