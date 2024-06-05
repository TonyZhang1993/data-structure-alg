[题目](https://leetcode.cn/problems/unique-paths-ii/description/)

一个机器人位于一个 m x n 网格的左上角 (起始点在下图中标记为 "Start" ). 

机器人每次只能向下或者向右移动一步. 机器人试图达到网格的右下角(在下图中标记为 "Finish"). 

现在考虑网格中有障碍物. 那么从左上角到右下角将会有多少条不同的路径?

网格中的障碍物和空位置分别用 1 和 0 来表示. 


```js
var uniquePathsWithObstacles = function(obstacleGrid) {
  const m = obstacleGrid.length
  const n = obstacleGrid[0].length
  let memo = Array.from({length: m}, () => Array(n).fill(0))

  //  dp 定义到 x,y 坐标 不同的路径数
  const dp = (x, y)  => {
    //  增加一个条件
    if (x < 0 || y < 0 || obstacleGrid[x][y] === 1) {
      return 0
    }

    if (x ==0 && y==0) {
      return 1
    }

    if (memo[x][y] !== 0) {
      return memo[x][y]
    }

    memo[x][y] = dp(x, y-1) + dp(x-1, y)
    return memo[x][y]

  }

  return dp(m-1, n-1)
};
```