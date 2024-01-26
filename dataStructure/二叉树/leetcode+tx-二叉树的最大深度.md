给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：

给定二叉树 `[3,9,20,null,null,15,7]，`

```
    3
   / \
  9  20
    /  \
   15   7

```

返回它的最大深度 3 。

## 思路
- 深度优先遍历DFS + 分治
- 一棵二叉树的最大深度等于左子树深度和右子树最大深度的最大值 + 1

```js 
//  递归 DFS
function maxDepth(root) {
  if (!root) {
    return 0
  } else {
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1
  }
}
//  参照 最小深度 的写法
var maxDepth = function(root) {
  if (!root) return 0
  if (!root.left && !root.right) return 1

  if (!root.left) {
    return maxDepth(root.right) + 1
  }

  if (!root.right) {
    return maxDepth(root.left) + 1

  }

  return Math.max(maxDepth(root.left),maxDepth(root.right)) + 1
};

//  迭代
function maxDepth(root) {
  if (!root) return 0
  //  用一个队列来存储每一层的节点
  let queue = [root]
  //  记录深度
  let depth = 0
  //  还存在节点
  while(queue.length > 0) {
    //  深度增加1，表示进入下一层
    depth++
    //  记录当前层的节点个数
    let levelSize = queue.length
    //  遍历当前层的每个节点
    for (let i=0; i<levelSize; i++) {
      //  弹出每个节点
      let node = queue.shift()
      //  如果当前节点有左子节点，将左子节点入队
      if (node.left) queue.push(node.left)
      if (node.right) queue.push(node.right)
    }
  }

  return depth
}
```