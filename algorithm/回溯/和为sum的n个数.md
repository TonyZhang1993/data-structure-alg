
给定无序, 不重复的数组data,取出 n 个数,使其相加和为sum

```js
function findSum(arr, n, sum) {
  let result = []
  /**
   * startIndex 当前下标
   * currentResult 当前结果
   * currentCombination 当前组合
   */
  const backtrack = (startIndex, currentResult, currentCombination) => {
    //  当前组合长度 等于 n  且 当前结果 等于 sum, 符合条件
    if (currentCombination.length === n && currentResult === sum) {
      result.push([...currentCombination])
      return
    }
    // 遍历startIndex开始的数
    for (let i=startIndex; i<arr.length; i++) {
      const num = arr[i]
      //  推入当前组合
      currentCombination.push(num)
      //  下标往后推移 
      backtrack(i + 1, currentResult + num, currentCombination)
      //  上面递归完后,回溯, 把num 推出
      currentCombination.pop()
    }
  }

  backtrack(0, 0, [])
  return result
}
```