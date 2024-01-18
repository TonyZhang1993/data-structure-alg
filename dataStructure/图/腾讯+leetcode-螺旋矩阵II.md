给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

```
输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```
https://leetcode.cn/problems/spiral-matrix-ii/description/

```js
/**
 * @param {number} n
 * @return {number[][]}
 */
var generateMatrix = function(n) {
  //  建立 n*n 的二维数组
  let result = []
  for (let i=0; i<n; i++) {
    result[i] = Array(n)
  }
  //  计数
  let cur = 1, max = n*n
  //  初始的边界情况
  let left = 0, right = n-1, top = 0, bottom = n-1

  while(cur<=max) {
    //  上面从左到右
    for (let i=left; i<=right; i++) {
      result[top][i] = cur
      cur++
    }
    top++

    //  右边从上到下
    for (let i=top; i<=bottom; i++) {
      result[i][right] = cur
      cur++
    }
    right--

    //  下面从右到左
    for (let i=right; i>=left; i--) {
      result[bottom][i] = cur
      cur++
    }
    bottom--
    
    //  左边从下到上
    for (let i=bottom; i>=top; i--) {
      result[i][left] = cur
      cur++
    }
    left++
  }

  return result
};
```