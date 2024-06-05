给你一幅由 N × N 矩阵表示的图像，其中每个像素的大小为 4 字节. 请你设计一种算法，将图像旋转 90 度. 

不占用额外内存空间能否做到?

[题目](https://leetcode.cn/problems/rotate-matrix-lcci/description/)
```
示例
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

## 思路
先将矩阵转置，再对每行进行reverse()
```
[                     
    [1,2,3],     
    [4,5,6],     =>   
    [7,8,9]
]

[
    [1,4,7],     
    [2,5,8],     =>   
    [3,6,9]
]

[
    [7,4,1],     
    [8,5,2],     =>   
    [9,6,3]
]
```
```js
var rotate = function(matrix) {
  const n = matrix.length
  //对角线反转
  for (let i=0; i<n; i++) {
    for (let j=0, j<i; j++) {
      swap(matrix, [i, j], [j, i])
    }
  }

  //  中线翻转
  for (let i=0; i<n; i++) {
    for (let j=0, j< n/2; j++) {
      swap(matrix, [i, j], [i, n-1-j])
    }
  }

  // matrix.forEach(item => {
  //   item.reverse()
  // })

  function swap(matrix, [x1, y1], [x2, y2]) {
    [matrix[x1, y1], matrix[x2,y2]] = [matrix[x2,y2],  matrix[x1,y1]]
  }
}

```
