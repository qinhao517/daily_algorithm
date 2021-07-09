# 删除链表的倒数第 N 个结点(2021-07-08)

## 题目描述

```text	
删除链表的倒数第 N 个结点:
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。
进阶：你能尝试使用一趟扫描实现吗？

示例 1：
    输入：head = [1,2,3,4,5], n = 2
    输出：[1,2,3,5]

示例 2：
    输入：head = [1], n = 1
    输出：[]

示例 3：
    输入：head = [1,2], n = 1
    输出：[1]

提示：
    链表中结点的数目为 sz
    1 <= sz <= 30
    0 <= Node.val <= 100
    1 <= n <= sz

本题来源：
    https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/
```



## 思路1:暴力解法

* 暴力解法就是第一时间想到的方法
* 可能不是最优的，但先解出来再说
* 时间/空间复杂度过大的时候，可以考虑优化

### java代码示例

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head == null){
            return null;
        }

        //遍历head，获取链表的节点个数
        //遍历之前需要把head存起来
        ListNode newHead = head;
        int count = 0;
        while(newHead != null){
            count++;
            newHead = newHead.next;
        }
    
        //定义一个新的链表
        ListNode newList = new ListNode(-1);
        //定义指针节点指向newList
        ListNode cur = newList;

        //遍历head
        //定义i变量来继续变量到哪个节点了
        int i = 1;
        while(head != null){
            if(i < (count - n + 1)){
                //说明没有遍历到指定删除的节点

                //将当前节点插入到新节点尾部
                cur.next = head;

                //当前节点移动到下一个节点
                head = head.next;

                //指针节点重新指向新的节点
                cur = cur.next;
                i++;
            }else{
                //说明到达指定删除的节点了，这时直接跳过该节点
                cur.next = head.next;

                //指针节点重新指向新的节点
                cur = cur.next;

                //当前节点移动到下一个节点
                head = head.next;

                i++;
            }
        }

        return newList.next;


    }
}
```

## 思路2:利用快慢指针

* 定义fast，slow两个快慢指针
* 先让fast指针移动n个节点，此时fast和slow节点相差n个节点
* 这个时候，对fast和slow指针同时一起遍历，当fast遍历到链表的尾部时，slow刚好指向到倒数第n个节点上
* 所以slow的下一个节点就是要删除的节点

### java代码示例

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head == null){
            return null;
        }

        //定义fast，slow两个快慢指针
        ListNode fast = head;
        ListNode slow = head;

        //先让fast指针移动n个节点
        int i = 1;
        while(fast != null && i < n){
            fast = fast.next;
            i++;
        }

        //对fast和slow指针同时一起遍历，当fast遍历到链表的尾部时，slow刚好指向到倒数第n个节点上
        //定义pre节点来暂时存放slow节点一定前的节点，这样直接去pre节点的next就是要删除的节点
        ListNode pre = null;
        while(fast.next != null){
            fast = fast.next;
            pre = slow;
            slow = slow.next;

        }

        if(pre == null){
            head = head.next;
        }else{
            pre.next = pre.next.next;
        }

        return head;

    }
}
```

