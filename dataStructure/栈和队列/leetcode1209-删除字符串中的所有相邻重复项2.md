[leetcode](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string-ii/description/)

```
给你一个字符串 s,「k 倍重复项删除操作」将会从 s 中选择 k 个相邻且相等的字母,并删除它们,使被删去的字符串的左侧和右侧连在一起. 

你需要对 s 重复进行无限次这样的删除操作,直到无法继续为止. 

在执行完所有删除操作后,返回最终得到的字符串. 
```

## 思路

解题思想:  遍历字符串依次入栈,入栈时,判断当前元素与栈头元素是否一致,如果不一致则入栈,如果一致,判断栈头字符是否长度为 `k - 1` ,如果为 `k-1` ,即加入该字符时,满足连续相同字符` k `个,此时,需要栈头出栈,当前字符不进栈,如果小于 `k-1` ,则取出栈头字符,加上当前字符,重新入栈

```js
var removeDuplicates = function(s, k) {
  let stack = []

  for (let i of s) {
    let prev = stack.pop()

    if (!prev || prev[0] !== i) {
      stack.push(prev)
      stack.push(i)
    } else if (prev.length < k - 1) {
      //  将相同的字符进行拼接,为后续统计数量准备
      stack.push(prev + i)
    }
  }

  return stack.join('')
};

O(n)
O(n)
```