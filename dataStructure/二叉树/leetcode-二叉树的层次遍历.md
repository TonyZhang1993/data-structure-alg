给你二叉树的根节点 root ,返回其节点值 自底向上的层序遍历 .  (即按从叶子节点所在层到根节点所在的层,逐层从左向右遍历)

[题目](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/description/)

```
    3
   / \
  9  20
    /  \
   15   7

结果: 
[
  [15,7],
  [9,20],
  [3]
]
```
```js
//  method1 - BFS 参照层序遍历,修改reverse() 即可
var levelOrderBottom= function(root) {
    if (!root) {
        return []
    }

    let res = [], queue = [root]

    while(queue.length) {
      let cur = [], tmp = []
      while(queue.length) {
        let node = queue.shift()
        cur.push(node.val)

        if(node.left) tmp.push(node.left)
        if(node.right) tmp.push(node.right)
      }

      res.push(cur)
      queue = tmp
    }

    return res.reverse()
}
O(n)
O(n)


//  method2 - DFS 
DFS 做本题的主要问题是:  DFS 不是按照层次遍历的. 为了让递归的过程中同一层的节点放到同一个列表中,在递归时要记录每个节点的深度 depth . 递归到新节点要把该节点放入 depth 对应列表的末尾. 

当遍历到一个新的深度 depth ,而最终结果 res 中还没有创建 depth 对应的列表时,应该在 res 中新建一个列表用来保存该 depth 的所有节点. 

const levelOrderBottom = function(root) {
  const res = []

  const dep = (node, depth) => {
    if (!node) return 
    //  如果为空,则准备好 []
    res[depth] = res[depth] || []
    res[depth].push(node.val)
    //  深度优先,就是继续带入左右节点 递归
    dep(node.left, depth+1)
    dep(node.right, depth+1)

  }

  dep(root, 0)

  return res.reverse()
}

O(n)
O(h)  //  h为高度
```

