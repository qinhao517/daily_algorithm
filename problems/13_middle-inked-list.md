# 链表的中间结点 解析

## 题目描述

```text
链表的中间结点：
给定一个头结点为 head 的非空单链表，返回链表的中间结点。
如果有两个中间结点，则返回第二个中间结点。

示例 1：
	输入：[1,2,3,4,5]
	输出：此列表中的结点 3 (序列化形式：[3,4,5])
	返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
	注意，我们返回了一个 ListNode 类型的对象 ans，这样：
	ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.

示例 2：
	输入：[1,2,3,4,5,6]
	输出：此列表中的结点 4 (序列化形式：[4,5,6])
	由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。

提示：
	给定链表的结点数介于 1 和 100 之间。

本题来源：
	https://leetcode-cn.com/problems/middle-of-the-linked-list/
```

## 思路1：暴力解法

* 先遍历链表获取到链表的长度len
* 根据len获取中间节点所在位置mid
* 根据mid获取中间节点
* 需要注意len的奇偶问题
* 思路简单，不展示具体实现代码了



## 思路2：双指针解法

* 定义一个快指针fast和一个慢指针slow
* 每次fast向前移动两个节点，slow移动一个节点
* 当fast节点或next节点为null时，说明slow移动到了链表的中间节点
* 此时输出第二个节点即可

## java代码示例

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
        if(head == null){
            return null;
        }

        //定义快慢指针
        ListNode fast = head,slow = head;

        //遍历链表，每次fast向前移动两个节点，slow移动一个节点
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
        }

        return slow;

    }
}
```

