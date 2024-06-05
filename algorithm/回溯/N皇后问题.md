[题目](https://leetcode.cn/problems/n-queens/description/)

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子. 

n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击. 

给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案. 

每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位. 


```js
/**
 * @param {number} n
 * @return {string[][]}
 */
var solveNQueens = function(n) {
  let res = []
  //  新建n*n 的二维数组，填充'.'
  let board = Array.from({length: n}, () => Array(n).fill('.'))

  //  回溯方法, row - 当前行
  let backtrack = (board, row) => {
    //  终止条件 - 如果当前行已经等于board 则结束
    if (row === board.length) {
      //  将二维的上的数组 转换成 字符串
      res.push(Array.from(board, row => row.join('')));
      return 
    }
    //  遍历选择
    for (let col=0; col<n; col++) {
      //  剪枝操作
      if (!isValid(board, row, col)) {
        continue
      }
      //  做选择
      board[row][col] = 'Q'
      backtrack(board, row + 1)
      //  撤销选择
      board[row][col] = '.'
    }

  }
  /**
   * 判断该点[row, col]是否可以放置 皇后
   */
  let isValid = (board, row, col) => {
    let n = board.length
    //  up
    for (let i=row; i>=0; i--) {
      if (board[i][col] === 'Q') {
        return false
      }
    }

    //  left-up
    for (let i=row-1, j=col-1; i>=0 && j>=0; i--, j--) {
      if (board[i][j] === 'Q') {
        return false
      }
    }
    //  right - up
    for (let i=row-1, j=col+1; i>=0 && j<n; i--, j++) {
      if (board[i][j] === 'Q') {
        return false
      }
    }

    return true
  }

  backtrack(board, 0)

  return res
};

//  isValid - 因为皇后是一行一行从上往下放的，所以左下方，右下方和正下方不用检查(还没放皇后）; 因为一行只会放一个皇后，所以每行不用检查. 也就是最后只用检查上面，左上，右上三个方向. 
// @lc code=end
```