[题目](https://leetcode.cn/problems/invert-binary-tree/description/)

给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。

```js
// 遍历+交换左右子树
// 解题思路： 从根节点开始依次遍历每个节点，然后交换左右子树既可

var invertTree = function(root) {

    if (!root) return null
    //  后序遍历
    if (root.left) invertTree(root.left)
    if (root.right) invertTree(root.right)
    let tmp = root.right 
    root.right = root.left
    root.left = tmp

    return root
};
```