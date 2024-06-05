## 题目

输入一颗二叉树的根节点和一个整数, 是否存在二叉树中路径结点值的和为输入整数. 路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径. 

说明: 叶子节点是指没有子节点的节点. 

## 思路

```js
思路一: DFS
> 只需要遍历整棵树

- 如果当前节点不是叶子节点, 递归它的所有子节点, 传递的参数就是 sum 减去当前的节点值; 
- 如果当前节点是叶子节点, 判断参数 sum 是否等于当前节点值, 如果相等就返回 true, 否则返回 false. 

const hasPathNum = (root, sum) => {
  if (!root) return false
  //  到头了, 无左右子树, 看当前的值是否相等
  if (!root.left && !root.right) {
    return root.val === sum
  }
  //  减去当前的值, 往下走是否有可能
  sum = sum - root.val
  return hasPathNum(root.left, sum) || hasPathNum(root.right, sum)
}
```