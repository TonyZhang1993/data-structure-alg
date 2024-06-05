输入一颗二叉树的根节点和一个整数,打印出二叉树中结点值的和为输入整数的所有路径. 路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径. 

```js
function findPath(root, target) {
  let result = []; // 存储所有符合要求的路径
  let path = []; // 存储当前路径
  function dfs(node, sum) {
    // 如果节点为空,返回
    if (!node) {
      return;
    }
    path.push(node.val); // 将当前节点加入路径
    sum += node.val; // 计算路径和
    // 如果是叶子节点且路径和等于目标值,将该路径加入结果数组中
    if (!node.left && !node.right && sum === target) {
      result.push([...path]); // 使用 spread operator 复制 path 数组,避免后续修改影响结果
    }
    dfs(node.left, sum); // 遍历左子树
    dfs(node.right, sum); // 遍历右子树
    path.pop(); // 回溯!!!!!!将当前节点从路径中删除
  }
  dfs(root, 0); // 从根节点开始遍历
  return result; // 返回结果数组
}

```