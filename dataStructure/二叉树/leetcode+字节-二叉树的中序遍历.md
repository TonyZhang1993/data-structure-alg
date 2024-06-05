中序遍历:  (左根右)

`前序(根左右),中序(左根右),后序(左右根)`
【这里的前, 中, 后 是以根节点作为参考！！！！】

根据前, 中遍历 / 后, 中遍历可以还原二叉树

`光有前序遍历和后序遍历是无法还原二叉树的. `

[题目](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

## 题目

给定一个二叉树,返回它的 中序 遍历. 

示例:
```
输入: [1,null,2,3]
   1
    \
     2
    /
   3
输出: [1,3,2]
```
进阶: 递归算法很简单,你可以通过迭代算法完成吗?

```js
//  递归实现, 两个参数
 var inorderTraversal = function (root, array = []) {
  if (root) {
    inorderTraversal(root.left, array)
    array.push(root.val)
    inorderTraversal(root.right, array)
  }

  return array
 }
//  递归实现, 一个参数
  var inorderTraversal = (root) => {
    let res = []
    const inOrder = (node) => {
      if (!node) return 
      inOrder(node.left)
      res.push(node.val)
      inOrder(node.right)
    }

    inOrder(root)
    return res
  }

 // 非递归
取根节点为目标节点,开始遍历
1.左孩子入栈 -> 直至左孩子为空的节点
2.节点出栈 -> 访问该节点
3.以右孩子为目标节点,再依次执行1, 2, 3

var inorderTraversal = function (root) {
  let result = []
  let stack = []

  let cur = root 
  while(cur || stack.length > 0) {
    //  左孩子入栈,直至左孩子为空的节点
    while(cur) {
      stack.push(cur)
      cur = cur.left
    }
    //  直至左孩子为空的节点
    cur = stack.pop()
    result.push(cur.val)
    cur = cur.right
  }

  return result
}

```