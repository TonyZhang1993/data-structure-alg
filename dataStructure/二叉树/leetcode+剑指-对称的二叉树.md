请实现一个函数,用来判断一颗二叉树是不是对称的. 注意,`如果一个二叉树同此二叉树的镜像是同样的,定义其为对称的`. 

## 思路
二叉树的右子树是二叉树左子树的`镜像二叉树`. 

`镜像二叉树: 两颗二叉树根结点相同,但他们的左右两个子节点交换了位置. `

![Alt text](对称二叉树.png)

如图,1为对称二叉树,2, 3都不是. 

- 两个根结点相等
- 左子树的左节点和右子树的右节点相同. 
- 左子树的右节点和右子树的左节点相同. 

递归所有节点满足以上条件即二叉树对称. 


```js
//  递归
// 左右子树的根节点值是否相等
// 左右子树是否镜像对称
function isSymmetrical(root) {

  const isSymmetricalTree = (node1, node2) => {
    if (!node1 && !node2) return true

    if (!node1 || !node2) return false

    if (node1.val !== node2.val) return false

    return isSymmetricalTree(node1.left, node2.right) && isSymmetricalTree(node1.right, node2.left)
  }

  //  查看 root 是否对称
  return isSymmetricalTree(root, root)
}
O(n)
O(n)

//  迭代
const isSymmetric = function(root) {
    if(!root) return true

    let stack = [root.left, root.right] 

    while(stack.length) {
      let right = stack.pop()
      let left = stack.pop()

      if (left && right) {
        if (left.val !== rigth.val) return false

        stack.push(left.left)
        stack.push(right.right)
        stack.push(left.right)
        stack.push(right.left)

      } else if (left || right){
        return false
      }
      
    }

    return true
}
```
