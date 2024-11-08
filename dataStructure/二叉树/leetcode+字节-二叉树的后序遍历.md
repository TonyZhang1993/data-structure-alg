后序遍历:  (左右根)

`前序(根左右),中序(左根右),后序(左右根)`
【这里的前, 中, 后 是以根节点作为参考！！！！】

根据前, 中遍历 / 后, 中遍历可以还原二叉树

`光有前序遍历和后序遍历是无法还原二叉树的. `

[题目](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)


## 题目

给定一个二叉树,返回它的 后序 遍历. 

示例:
```
输入: [1,null,2,3]
   1
    \
     2
    /
   3
输出: [3,2,1]
```
进阶: 递归算法很简单,你可以通过迭代算法完成吗?

```js
//  递归实现
 var postorderTraversal = function (root, array = []) {
  if (root) {
    postorderTraversal(root.left, array)
    postorderTraversal(root.right, array)
    array.push(root.val)

  }

  return array
 }

 // 非递归
取根节点为目标节点,开始遍历
1.左孩子入栈 -> 直至左孩子为空的节点
2.栈顶节点的右节点为空或右节点被访问过 -> 节点出栈并访问他,将节点标记为已访问
3.栈顶节点的右节点不为空且未被访问,以右孩子为目标节点,再依次执行1, 2, 3

var postorderTraversal = function (root) {
  let result = []
  let stack = []
  let last = null //  标记上一个访问的节点

  let cur = root 
  while(cur || stack.length > 0) {
    //  左孩子入栈,直至左孩子为空的节点
    while(cur) {
      stack.push(cur)
      cur = cur.left
    }
    cur = stack[stack.length - 1]
    //  栈顶节点的右节点为空或右节点被访问过 !!!!**关键部分**
    if (!cur.right || cur.right === last) {
      cur = stack.pop()
      result.push(cur.val)
      last = cur
      cur = null // 置空, 进入下一个循环, 继续弹栈
    } else {
      cur = cur.right
    }
  }

  return result
}

```