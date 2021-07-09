# 环形链表 解析(2021-07-06)

## 题目描述

```text
环形链表：
给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 

示例 1：
    输入：head = [3,2,0,-4], pos = 1
    输出：true
    解释：链表中有一个环，其尾部连接到第二个节点

示例 2：
    输入：head = [1,2], pos = 0
    输出：true
    解释：链表中有一个环，其尾部连接到第一个节点。

示例 3：
    输入：head = [1], pos = -1
    输出：false
    解释：链表中没有环。

提示：
    链表中节点的数目范围是 [0, 104]
    -105 <= Node.val <= 105
    Pos 为 -1 或者链表中的一个 有效索引 。

本题来源：
链接：https://leetcode-cn.com/problems/linked-list-cycle
```



## 思路：快慢指针找到是否有环

* 定义两个指针，一个快指针，一个慢指针
* 快指针一次走两步，慢指针一次走一步
* 遍历链表，直到fast与slow再次相遇，表示链表有环
* 时间复杂度：O(n)
* 空间复杂度：O(1)

## java代码示例

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
    public boolean hasCycle(ListNode head) {
        //定义两个指针，一个快指针，一个慢指针
        ListNode fast = head, slow = head;

        //遍历链表，直到fast与slow再次相遇，表示链表有环
        while(fast != null && fast.next != null){
            fast = fast.next.next;//快指针每次移动两个节点
            slow = slow.next;//慢指针每次移动一个节点

            //有环的话，fast肯定会与slow再次相遇
            if(fast == slow){
                return true;
            }
        }

        return false;

    }
}
```

