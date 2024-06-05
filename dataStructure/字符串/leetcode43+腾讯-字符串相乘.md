[leetcode43](https://leetcode.cn/problems/multiply-strings/description/)

给定两个以字符串形式表示的非负整数 num1 和 num2,返回 num1 和 num2 的乘积,它们的乘积也表示为字符串形式. 

注意: 不能使用任何内置的 BigInteger 库或直接将输入转换为整数. 

 
```
示例 1:
输入: num1 = "2", num2 = "3"
输出: "6"

示例 2:
输入: num1 = "123", num2 = "456"
输出: "56088"
```
## 思路



## 题解
```js
//  字符串相乘; 
var multiply = function(num1, num2) {
  if(num1 === '0' || num2 ==='0') return '0';
  //  记录字符串长度
  let m = num1.length, n = num2.length;
  //  创建可能最大的位数,用0填充
  let res = new Array(m+n).fill(0);
  //  从num1 最低位开始
  for(let i = m - 1; i >= 0; i--){
    //  处理为数字
    let n1 = num1[i] - '0';
    //  从num2最低位开始
    for(let j = n - 1; j >= 0; j--){
      //  处理为数字
      let n2 = num2[j] - '0';
      //  得到乘积,以及可能的进位
      let inSize = res[i+j+1] + n1*n2;
      //  获得该位的值
      res[i+j+1] = inSize % 10;
      //  设置进位,注意是累加的效果！！！！
      res[i+j] += Math.floor(inSize/10)
    }
  }
  //  转为字符串 并去除开头可能存在的0
  return res.join('').replace(/^0*/g, '');
};
O(m*n)
O(m+n)

```