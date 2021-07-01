## 19. 删除链表的倒数第 N 个结点
[网址](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

* 思路1（自己的实现）
遍历一遍得到 length，第二次从头走 length - N

* 思路2
两个指针 fast slow, fast先走n步，然后fast和slow一起走

* 思路3
递归思路，recursive(head.next, N), 递归第N次


## 24. 两两交换链表中的节点
[网址](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

* 思路1 （自己的实现）

解这种规律题，尽量用中间的节点来演算思路，因为头节点和尾节点都是边界值，不利于演算规律。

![图片](./resources/linkedlist_1.png)

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

**递归** 

感觉现在没有形成递归的思想，所以总是想不起来用递归。不过递归需要压栈，如果数据无穷，有溢出风险。
[递归参考博客](https://lyl0724.github.io/2020/01/25/1/)


```
// @mata川
//
// 使用递归来解决该题，主要就是递归的三部曲：
//
// 找终止条件：本题终止条件很明显，当递归到链表为空或者链表只剩一个元素的时候，没得交换了，自然就终止了。
// 找返回值：返回给上一层递归的值应该是已经交换完成后的子链表。
// 单次的过程：因为递归是重复做一样的事情，所以从宏观上考虑，只用考虑某一步是怎么完成的。我们假设待交换的俩节点分别为head和next，next的应该接受上一级返回的子链表(参考第2步)。
// 就相当于是一个含三个节点的链表交换前两个节点，就很简单了，想不明白的画画图就ok。
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode next = head.next;
        head.next = swapPairs(next.next);
        next.next = head;
        return next;
    }
}
```
## 61. 旋转链表
[网址](https://leetcode-cn.com/problems/rotate-list/)

* 思路1

遍历一遍得到length， k % length 得到真正的移动个数

思路1的结果不好，执行用时：108 ms, 在所有 JavaScript 提交中击败了26.27%的用户， 内存消耗：39.8 MB, 在所有 JavaScript 提交中击败了11.52%的用户
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
 * @param {number} k
 * @return {ListNode}
 */
var rotateRight = function(head, k) {
    if (!head || !head.next) return head;

    let length = 1;
    let current = head;
    while(current.next) {
        length++
        current = current.next;
    }
    
    let step = k % length;
    if (step === 0) {
        return head;
    }

    current.next = head;    // 构成循环链
    current = head.next;
    for (let i = 0; i < length - step - 1; i++) {
        current = current.next;
        head = head.next;
    }
    head.next = null;
    return current;
};
```

* 思路2

想起来前几天做的两个指针fast 和 slow，感觉是一个思路。
最后优化了下，发现整体还是一个思路。
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
 * @param {number} k
 * @return {ListNode}
 */
var rotateRight = function(head, k) {
    if (!head || !head.next || k === 0) return head;

    let length = 1;
    let fast = head;
    while (fast.next) {
        fast = fast.next;
        length++;
    }

    fast.next = head; // 循环
    let step = length - k % length;
    while(step > 1) {
        head = head.next;
        step--;
    }
    fast = head.next;
    head.next = null;
    return fast;
};
```
## 82. 删除排序链表中的重复元素 II
[网址](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

* 思路1

我看错题了，题目要求重复的元素全部移除，我理解成重复的元素留一个，谨慎！！！

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
var deleteDuplicates = function(head) {
    if (!head || !head.next) return head;

    let max = head.val;
    let current = head;
    while (current.next) {
        if (max === current.next.val) {
            current.next = current.next.next;
            continue;
        }
        max = current.next.val;
        current = current.next;
    }
    return head;
};
```

* 思路2

一直没有做出来，后来看了答案，sad～
总结一下，自己被排序和max值的思路限制住了，一直想用一个指针搞定，但是这种就会丢失前序指针，导致删除时不能完全删除。
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
var deleteDuplicates = function(head) {
    let current = new ListNode(Number.MIN_VALUE, head);
    head = current; // 保留头节点 避免被删

    let left, right;
    while (current.next) {
        left = right = current.next;
        while (right.next && left.val === right.next.val) {
            right = right.next;
        }

        if (left === right) current = current.next;
        else current.next = right.next;
    }
    return head.next;
};
```
* 思路3 递归

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
var deleteDuplicates = function(head) {
    if (!head || !head.next) return head; 

    if (head.val !== head.next.val) {
        head.next = deleteDuplicates(head.next);
    } else {
        right = head.next;
        while(right && right.val === head.val) {
            right = right.next;
        }
        head = deleteDuplicates(right);
    }
    return head;
};
```

## 2. 两数相加
[网址](https://leetcode-cn.com/problems/add-two-numbers/)

* 思路1

思路1比较直白，从链表头，也就是低位开始相加，然后考虑进位。
```
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    let head = new ListNode(0, null);
    let current = head;
    let nextBaseVal = 0;
    while(l1 || l2 || (nextBaseVal !== 0 )) {
        let item = new ListNode(0, null);
        current.next = item;
        current = current.next;

        const sum = (l1 ? l1.val : 0) + (l2 ? l2.val : 0) + nextBaseVal; 
        current.val = sum % 10;
        nextBaseVal = sum <= 9 ? 0 : parseInt(sum / 10);
        
        l1 = l1 ? l1.next : null;
        l2 = l2 ? l2.next : null;
    }
    return head.next;
};
```
* 思路2 - 递归

思考递归解法的时候差一点就成功了。。。。 加油！
```
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    return addTwoNumbers2(l1, l2, 0);
};

var addTwoNumbers2 = function(l1, l2, nextVal) {
    // 结束条件
    if (!l1 && !l2) {
        return nextVal === 0 ? null : new ListNode(nextVal, null);
    }

    const sum = (l1 ? l1.val : 0) + (l2 ? l2.val : 0) + nextVal;
    return new ListNode(sum % 10, addTwoNumbers2((l1 ? l1.next : null), (l2 ? l2.next : null), parseInt(sum / 10)));
}
```

## 86. 分隔链表
[网址](https://leetcode-cn.com/problems/partition-list/)

* 思路

题目描述的不是很清楚，看了好几遍才明白。 整体就是把所有小于x的节点按顺序组成新的链表，然后再尾部再追加剩下的节点链表。
搞笑的是发现运行用例后的时间和空间数据有问题，一样的代码第一次跑时间只超过19%的，第二次运行超过90%，即使有用例的区别，这个数据也是奇怪了。

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
 * @param {number} x
 * @return {ListNode}
 */
var partition = function(head, x) {
    if (!head || !head.next) return head;

    let current = head = new ListNode(0, head);
    let anotherCurrent = anotherHead = new ListNode(0, null);

    while (current.next) {
        if (current.next.val < x) {
            anotherCurrent.next = current.next;
            anotherCurrent = anotherCurrent.next;
            current.next = current.next.next;
            anotherCurrent.next = null;
        } else {
            current = current.next;
        }
    }

    // splice
    anotherCurrent.next = head.next;
    return anotherHead.next;
};
```

## 23. 合并K个升序链表
[网址](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

* 思路1

比较直白的思路就是，每次取k个链表的头部节点的最小值节点。今天做的是困难的级别哦~
```
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
 var mergeKLists = function(lists) {
  let current = head = new ListNode(0, null);

  while(lists && lists.length > 0) {
    let {list, index} = minKLists(lists);
    if (!list) {
        return head.next;
    }

    current.next = list;
    if (list.next) {
      lists[index] = list.next;
    } else {
      lists.splice(index, 1);
      if (lists.length === 0) {
        lists = null;
      }
    }
    current.next.next = null;
    current = current.next;
  }

  return head.next;
};

// get min list
var minKLists = function(lists) {
  if (!lists || lists.length === 0) return null;

  let minPoint;
  let minIndex;
  lists.forEach((element, index) => {
    if (!element || element.length === 0) {
        return;
    }
    if (!minPoint) {
      minPoint = element;
      minIndex = index;
      return;
    }
    if (minPoint.val > element.val) {
      minPoint = element;
      minIndex = index;
    }
  });
  return { list: minPoint, index: minIndex };
}
```

## 25. K 个一组翻转链表
[网址](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

* 思路1 

比较直白和暴力，这道题整体两个单元策略，1是找到k个节点，2是反转k个节点。 但是显然这个的效率有点低，结果不是很理想。

执行用时：184 ms, 在所有 JavaScript 提交中击败了5.27%的用户内存消耗：45 MB, 在所有 JavaScript 提交中击败了5.06%的用户
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
 * @param {number} k
 * @return {ListNode}
 */
var reverseKGroup = function(head, k) {
    console.log(head, k);

    if (k < 2) return head;
    if (!head || !head.next) return head;

    let current = new ListNode(0, head);
    head = current;

    while (current.next) {
        console.log('current', current);

        // find first k
        const {start: kStart, end: KEnd} = getFirtKGroup(current.next, k);
        if (!kStart || !KEnd) return head.next;
        // console.log('getFirstKGroup finished', current, kStart.val, KEnd.val);

        // reverse
        const {head: KHead, tail: kTail} = reverse(kStart, KEnd);
        // console.log('reverse finished', 'head', KHead, 'tail', kTail);

        // splice
        current.next = KHead;
        current = kTail;
        // console.log('head', head);
    }
    
    return head.next;
};

/**
 * get first k group, return {start pointer, end pointer}
 * if less than k, return {start pointer, end pointer is null}
 */
var getFirtKGroup = function(head, k) {
    let start = end = head;
    let index = k;
    while(index > 1 && end) {
        index--;
        end = end.next;
    }
    if (index === 1) return {start, end};
    return {start, end: null};
}

/**
 * reverse list, return new head and tail
 */
var reverse = function(head, tail) {
    if (!head || !head.next) return { head, tail: head};

    let current = head;
    let newHead = new ListNode(0, null);
    let newTail;
    let end = tail.next;

    while (current !== end) {
        let temp = current;
        current = current.next;
        temp.next = newHead.next;
        newHead.next = temp; 
        
        if (!newTail) {
            newTail = temp;
            newTail.next = end;
        }
    }
    
    return {'head': newHead.next, 'tail': newTail};
}
```

* 思路2 

在思路1上的优化，思路1需要先遍历k， 然后反转k， 这里统一成一步，遍历k的同时进行翻转。
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
 * @param {number} k
 * @return {ListNode}
 */
var reverseKGroup = function(head, k) {
    console.log(head, k);

    let dummy = new ListNode(0, head);
    let length = 0;
    while(head) {
        length++;
        head = head.next;
    }
    console.log('length', length);

    head = dummy.next;
    let prev = dummy;
    let curr = head;
    let next;
    for(let i = 0; i< parseInt(length / k); i++) {

        let reverseDummy = new ListNode(0, null);
        for (let j = 0; j < k; j++) {
            next = curr.next;
            curr.next = reverseDummy.next;
            reverseDummy.next = curr;
            curr = next;
        }
        console.log('reverseDummy', reverseDummy);

        prev.next.next = next;
        curr = prev.next;
        prev.next = reverseDummy.next;
        prev = curr;
        curr = curr.next;
    }
    return dummy.next;
};
```
