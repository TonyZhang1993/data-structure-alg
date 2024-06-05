[leetcode](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)

```
输入: "abbaca"
输出: "ca"
解释: 
例如,在 "abbaca" 中,我们可以删除 "bb" 由于两字母相邻且相同,这是此时唯一可以执行删除操作的重复项. 之后我们得到字符串 "aaca",其中又只有 "aa" 可以执行重复项删除操作,所以最后的字符串为 "ca". 
```

```js
var removeDuplicates = function(s) {
  let stack = []

  for (let i=0; i<s.length; i++) {
    if (stack.length === 0) {
      stack.push(s[i])
    } else if(stack[stack.length - 1] === s[i]) {
      stack.pop()
    } else {
      stack.push(s[i])
    }
  }

  //  优化 stack = [s[0]] 先把第一位加进去,后面就不用判断stack 长度

  return stack.join('')
};

O(n)
O(n)
```