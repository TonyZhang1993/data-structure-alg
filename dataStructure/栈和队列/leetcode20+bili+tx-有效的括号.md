
[leetcode](https://leetcode.cn/problems/valid-parentheses/description/)

## 思路

解题思路:  将字符串中的字符依次入栈,遍历字符依次判断: 

- 首先判断该元素是否是` { 、 ( 、 [ `,直接入栈
- 否则该字符为 `} 、 ) 、 ] `中的一种,如果该字符串有效,则该元素应该与栈顶匹配,例如栈中元素有 `({`, 如果继续遍历到的元素为 `)`, 那么当前元素序列为` ({) `是不可能有效的,所以此时与栈顶元素匹配失败,则直接返回 false ,字符串无效
- 当遍历完成时,所有已匹配的字符都已匹配出栈,如果此时栈为空,则字符串有效,如果栈不为空,说明字符串中还有未匹配的字符,字符串无效

```js
var isValid = function(s) {
  let map = {
    '{': '}',
    '(': ')',
    '[': ']'
  }

  let stack = []

  for (let i=0; i<s.length; i++) {
    if (map[s[i]]) {
      stack.push(s[i])
      //  注意条件:  在条件中已经pop
    } else if (map[stack.pop()] !== s[i]) {
      return false
    }

    // stack.pop()
  }

  return stack.length === 0
};

O(n)
O(n)

```