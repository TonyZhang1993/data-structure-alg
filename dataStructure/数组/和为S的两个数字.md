## 题目

输入一个递增排序的数组和一个数字`S`,在数组中查找两个数,使得他们的和正好是`S`,如果有多对数字的和等于S,输出两个数的乘积最小的. 

## 思路

- 数组中可能有多对符合条件的结果,而且要求输出乘积最小的,说明要分布在两侧 比如 3,9 5,7 要取3,9. 
- 数组是有序的,可以使用使用大小指针求解,不断逼近结果,最后取得最终值. 

```js
function findSumArr(arr, sum) {
  if (arr && arr.length > 1) {

    const length = arr.length
    let start = 0
    let end = length - 1

    while(start < end) {
      let total = arr[start] + arr[end]

      if (total < sum) {
        start ++
      } else if (total] === sum) {
        return [arr[start], arr[end]]
      } else {
        end --
      }
    }
  }
  return []
}
```