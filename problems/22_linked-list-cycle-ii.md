#### 环形链表 II 解析

## 题目描述

```text
给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。
说明：不允许修改给定的链表。
进阶：
你是否可以使用 O(1) 空间解决此题？

示例 1：
  输入：head = [3,2,0,-4], pos = 1
  输出：返回索引为 1 的链表节点
  解释：链表中有一个环，其尾部连接到第二个节点。
  
示例 2：
	输入：head = [1,2], pos = 0
  输出：返回索引为 0 的链表节点
  解释：链表中有一个环，其尾部连接到第一个节点。
  
示例 3：
	输入：head = [1], pos = -1
  输出：返回 null
  解释：链表中没有环。
  
提示：
* 链表中节点的数目范围在范围 [0, 104] 内
* -105 <= Node.val <= 105
* pos 的值为 -1 或者链表中的一个有效索引

题目来源：https://leetcode-cn.com/problems/linked-list-cycle-ii/
```

## 思路：

* 利用快慢指针，快指针一次移动两个节点，慢指针一次移动一个节点
* 当判断链表有环时：
  * 此时把快指针重新放到起点，改为一次移动一个节点，慢指针继续移动
  * 当两个指针相遇时，就是环的第一个节点

## 代码示例

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        //判断head节点是否为null
        if(head == null || head.next == null){
            return null;
        }

        //定义两个快慢指针
        ListNode fastNode = null;
        ListNode slowNode = null;

        //开始时，都指向head头节点
        fastNode = slowNode = head;
        //快指针一次移动两个节点，慢指针一次移动一个节点，当两个指针第一次相遇时，说明有环
        int flag = 0;//用来标识是否有环
        while(fastNode != null && fastNode.next != null){
            //开始移动
            fastNode = fastNode.next.next;
            slowNode  = slowNode.next;

            //节点相等时，说明有环，此时为第一次相遇，退出循环
            if(fastNode == slowNode){
                flag = 1;
                break;
            }
        }
        
        if(flag == 1){
            //有环，此时把快指针重新放到起点，改为一次移动一个节点，慢指针继续一定，当两个指针相遇时，就是环的第一个节点
            fastNode = head;
            while(fastNode != slowNode){
                fastNode = fastNode.next;
                slowNode = slowNode.next;
            }
            return fastNode;
        }

        return null;
    }
}
```

