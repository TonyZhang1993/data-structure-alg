[题目](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先. (一个节点也可以是它自己的祖先)

说明:

所有节点的值都是唯一的. 
p, q 为不同节点且均存在于给定的二叉树中. 

```
        3
      /   \
     5     1
    / \   / \
   6   2 0   8
      / \
     7   4

p = 5, q = 1  输出: 3

p = 5, q = 4  输出: 5
```

## 解题思路

如果树为空树或 p ,  q 中任一节点为根节点,那么 p ,  q 的最近公共节点为根节点

如果不是,即二叉树不为空树,且 p ,  q 为非根节点,则递归遍历左右子树,获取左右子树的最近公共祖先,

- 如果 p ,  q 节点在左右子树的最近公共祖先都存在,说明 p ,  q 节点分布在左右子树的根节点上,此时二叉树的最近公共祖先为 root
- 若 p ,  q 节点在左子树最近公共祖先为空,那 p , q 节点位于右子树上,最终二叉树的最近公共祖先为右子树上 p , q 节点的最近公共祖先
- 若 p ,  q 节点在右子树最近公共祖先为空,同左子树 p ,  q 节点的最近公共祖先为空一样的判定逻辑
- 如果 p ,  q 节点在左右子树的最近公共祖先都为空,则返回 null


```js
// function TreeNode(val) {
//   this.val = val;
//   this.left = null;
//   this.right = null;
// }

const lowestCommonAncestor = function(root, p, q) {
  //  如果树为空树或 p ,  q 中任一节点为根节点,那么 p ,  q 的最近公共节点为根节点
  if (root === null || root === p || root === q) {
    return root
  }
  //  在左子树查找p, q的最近公共祖先
  const left = lowestCommonAncestor(root.left, p, q)
  //  在右子树查找p, q的最近公共祖先
  const right = lowestCommonAncestor(root.right, p, q)
  //  如果在左子树找不到,那么就在右子树上
  if (!left) return right
  //  如果在右子树找不到,那么就在左子树上
  if (!right) return left
  //  如果都找不到,就是root 了
  return root
}
O(n)
O(n)
```