## 题目

阿里算法题: 编写一个函数计算多个数组的交集

要求: 
输出结果中的每个元素一定是唯一的


## 思路



```js

const findOverLap = (...arrs) => {
  if (arrs.length === 0) return []
  if (arrs.length === 1) return arrs[0]
  return [...new Set(arrs.reduce((total, current) => {
    return current.filter(item => total.includes(item))
  }))]
}

```
