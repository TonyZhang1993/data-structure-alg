## 题目

输入一个正数S，打印出所有和为S的连续正数序列。

例如：输入15，有序1+2+3+4+5 = 4+5+6 = 7+8 = 15 所以打印出3个连续序列1-5，5-6和7-8。

## 思路
- 创建一个子容器 child， 用于表示当前的子序列，初始元素是1,2
- 记录子序列的开头元素 small和 末尾元素 big
- big 向右移动子序列末尾增加一个数；small向右移动子序列首部移除一个数
- 当子序列的和大于目标值，small 向右移动； 当子序列的和小于目标值，big向左移动

```js
function findContinueSequence(sum) {
  const child = [1, 2]
  let small = 1
  let big = 2
  let currentSum = 3
  let result = []
  
  while (big < sum) {
    while (currentSum < sum && big < sum) {
      //  注意先自加，再push
      child.push(++big)
      currentSum += big
    }
    //  注意判断条件
    while (currentSum > sum && small < big) {
      child.shift() //  移除头部元素
      currentSum -= small++
    }
    //  注意判断条件
    if (currentSum === sum && child.length > 1) {
      //  复制数组
      result.push(child.slice())
      //  进入后面一轮
      child.push(++big)
      currentSum += big
    }
  }

  return result
}
```