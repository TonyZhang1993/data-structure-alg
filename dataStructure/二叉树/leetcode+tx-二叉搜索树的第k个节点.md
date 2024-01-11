给定一棵二叉搜索树，请找出其中的第k小的结点。 例如， （5，3，7，2，4，6，8） 中，按结点数值大小顺序第三小结点的值为4。

tip: 二叉搜索树 - 在左侧储存比父节点小的值，在右侧储存比父节点大的值。

```
       5
    /    \
   3      7
  / \    / \
 2   4  6   8
```

https://leetcode.cn/problems/kth-smallest-element-in-a-bst/description/

## 思路
二叉搜索树的`中序遍历`即排序后的节点，本题实际考察二叉树的遍历。

上述中序遍历：2 - 3 - 4 - 5 - 6 - 7 - 8
前序：5 - 3 - 2 - 4 - 7 - 6 - 8
后序： 2 - 4 - 3 - 6 - 8 - 7 - 5

本质就是 考察 二叉树`中序遍历`，加一个k 参数判断！！！！


```js
//  递归实现 
function findKth(root, k) {
  let res = []

  const inOrder = (node) => {
    if (!node) return 

    inOrder(node.left)
    res.push(node.val)
    inOrder(node.right)
  }

  inOrder(root)

  let len = res.length
  //  注意条件
  if (k > 0 && k <= len) {
    return res[k - 1]
  } else {
    return null
  }
}

//  优化调整 - 不要完整得到 中序遍历结果； 可以中途截胡！！！提高效率
//  优化后!!!!
function findKth(root, k) {
  let res = null

  const inOrder = (node) => {
    if (!node || k<1) return 

    inOrder(node.left)
    //  以下内容关键！！！！！！
    if (--k === 0) {
      res = node.val
      return
    }
    inOrder(node.right)
  }

  inOrder(root)

  return res
}
时间复杂度：O(k)
空间复杂度：不考虑递归栈所占用的空间，空间复杂度为 O(1)

//  非递归实现
function findKth(root, k) {
  let stack = []
  let cur = root

  while(cur || stack.length > 0) {

    while(cur) {
      stack.push(cur)
      cur = cur.left
    }

    cur = stack.pop()
    //  以下内容关键！！！！！！
    if (--k === 0) {
      return node.val
    }
    cur = cur.right
  }

  return null

}
```