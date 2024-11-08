前序遍历:  (根左右)

`前序(根左右),中序(左根右),后序(左右根)`
【这里的前, 中, 后 是以根节点作为参考！！！！】

根据前, 中遍历 / 后, 中遍历可以还原二叉树

`光有前序遍历和后序遍历是无法还原二叉树的. `

[题目](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)


## 题目

给定一个二叉树,返回它的 前序 遍历. 

示例:
```
输入: [1,null,2,3]
   1
    \
     2
    /
   3
输出: [1,2,3]
```
进阶: 递归算法很简单,你可以通过迭代算法完成吗?

```js
//  递归实现
 var preorderTraversal = function (root, array = []) {
  if (root) {
    array.push(root.val)
    preorderTraversal(root.left, array)
    preorderTraversal(root.right, array)
  }

  return array
 }

 // 非递归
取根节点为目标节点,开始遍历
1.访问目标节点
2.左孩子入栈 -> 直至左孩子为空的节点
3.节点出栈,以右孩子为目标节点,再依次执行1, 2, 3

var preorderTraversal = function (root) {
  let result = []
  let stack = []

  let cur = root 
  while(cur || stack.length > 0) {
    //  左孩子入栈,直至左孩子为空的节点
    while(cur) {
      result.push(cur.val)
      stack.push(cur)
      cur = cur.left
    }
    //  直至左孩子为空的节点
    cur = stack.pop()
    cur = cur.right
  }

  return result
}

```