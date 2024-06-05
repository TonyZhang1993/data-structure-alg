数字 n 代表生成括号的对数,请你设计一个函数,用于能够生成所有可能的并且 有效的 括号组合. 

[题目](https://leetcode.cn/problems/generate-parentheses/solutions/426794/gua-hao-sheng-cheng-by-user7746o/)

```js
var generateParenthesis = function(n) {
  let res = []
  let leftSum = 0
  let rightSum = 0

  const backtrack = (track) => {
    //  剪枝
    if (leftSum > n || rightSum > leftSum) return
    //  终止条件
    if (leftSum === n && rightSum === n && track.length === n*2) {
      res.push(track.join(''))
      return
    }
    //  添加左括号
    track.push('(')
    leftSum++
    backtrack(track)
    leftSum--
    track.pop()

    //  添加右括号
    track.push(')')
    rightSum++
    backtrack(track)
    rightSum--
    track.pop()
  }

  backtrack([])
  return res
};


//  方法二 - 思想和上面差不多
const generateParenthesis = (n) => {
  const res = []
  const dfs = (path, left, right) => {
    // 肯定不合法,提前结束
    if (left > n || left < right) return
    // 到达结束条件
    if (left + right === 2 * n) {
        res.push(path)
        return
    }
    // 选择
    dfs(path + '(', left + 1, right)
    dfs(path + ')', left, right + 1)
  }
  dfs('', 0, 0)
  return res
}
```

