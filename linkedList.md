## 删除链表的倒数第 N 个结点
[网址](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

* 思路1
遍历一遍得到 length，第二次从头走 length - N

* 思路2
两个指针 fast slow, fast先走n步，然后fast和slow一起走

* 思路3
递归思路，recursive(head.next, N), 递归第N次


## 两两交换链表中的节点
[网址](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

* 思路1

```
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var swapPairs = function(head) {
    if (!head || !head.next) {
        return head;
    }
    const newHead = new ListNode(0, head);

    let current = newHead;
    let last;
    while(current.next  && current.next.next) {
        last = current.next;
        current.next = current.next.next;
        last.next = current.next.next;
        current.next.next = last;

        current = last;
    }

    return newHead.next;
};
```

* 思路2
