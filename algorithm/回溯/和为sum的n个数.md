给定无序、不重复的数组data，取出 n 个数，使其相加和为sum


```js
function findSum(arr, n, sum) {
  let result = []

  const backtrack = (startIndex, currentResult, currentCombination) => {
    if (currentCombination.length === n && currentResult === sum) {
      result.push([...currentCombination])
      return
    }

    for (let i=startIndex; i<arr.length; i++) {
      const num = arr[i]
      
      // if (currentResult)
      currentCombination.push(num)

      backtrack(i+1, currentResult+num, currentCombination)
      currentCombination.pop()
    }
  }

  backtrack(0, 0, [])
  return result
}
```