[题目](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

## 题目
给你二叉树的根节点 root ，返回其节点值的 层序遍历 .  (即逐层地，从左到右访问所有节点）. 

```
    3
   / \
  9  20
    /  \
   15   7

结果: 
[
  [3],
  [9,20],
  [15,7]
]
```

```js
//  解法一: BFS(广度优先遍历）

// BFS 是按层层推进的方式，遍历每一层的节点. 题目要求的是返回每一层的节点值，所以这题用 BFS 来做非常合适. 
// BFS 需要用队列作为辅助结构，我们先将根节点放到队列中，然后不断遍历队列. 

const levelOrder = (root) => {
  if (!root) {
    return []
  }
  //  queue 存放当前层级所有的node
  let res = [], queue = [root]

  while(queue.length > 0) {
    //  cur当前层级 各 节点的值， tmp 存放当前层级节点 的 左右子节点
    let cur = [], tmp =[]

    while(queue.length>0) {
      //  从左弹出一个节点
      let node = queue.shift()
      //  记录值
      cur.push(node.val)
      //  若有左右子树，推入tmp中
      if (node.left) tmp.push(node.left)
      if (node.right) tmp.push(node.right)
    }
    //  推入各层级记录的值
    res.push(cur)
    //  赋值当前层级的左右子树列表
    queue = tmp
  }

  return res

}

//  method2 - DFS 

const levelOrder = (root) => {
  let res = []
  //  depth 当前层级
  const dep = (node, depth) => {
    if (!node) return

    res[depth] = res[depth] || []
    res[depth].push(node.val)

    dep(node.left, depth + 1)
    dep(node.right, depth + 1)
  } 

  dep(root, 0)

  return res
}

