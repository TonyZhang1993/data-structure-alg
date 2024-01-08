https://leetcode.cn/leetbook/read/linked-list/jf1cc/

## 删除链表的倒数第N个节点

说明：

给定的 n 保证是有效的。

进阶：

你能尝试使用一趟扫描实现吗？

```js
//  快指针先行
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 * 
 */
var removeNthFromEnd = function(head, n) {
  //  创建一个节点，作为head 前置节点
  let preHead = new ListNode(0)

  let preHead.next = head
  //  创建快慢节点
  let slow = preHead, fast = preHead
  //  根据n 让fast 先行
  while(n--) {
    fast = fast.next
  }
  //  注意条件：直到fast为最后一个节点
  while (fast && fast.next) {
    fast = fast.next
    slow = slow.next
  }
  //  跳过要删除的节点
  slow.next = slow.next.next
  //  返回头节点
  return preHead.next
}

```