# 反转链表 解析(2021-07-05)

## 题目描述

```text	
反转链表：
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表

示例 1：
	输入：head = [1,2,3,4,5]
	输出：[5,4,3,2,1]

示例 2：
	输入：head = [1,2]
	输出：[2,1]

示例 3：
	输入：head = []
	输出：[]

提示：
	链表中节点的数目范围是 [0, 5000]
	-5000 <= Node.val <= 5000
```



## 思路:利用指针进行节点转换

* 定义两个指针，cur和pre指针
* 具体的步骤，可以参考我画的图
* 图解![](/Users/apple/Downloads/WechatIMG658.png)

#### java代码示例

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
    public ListNode reverseList(ListNode head) {
        //初始化当前节点，前置节点
        ListNode cur = head,pre = null;

        //当前节点不为null时，遍历节点
        while(cur != null){
            //暂存后继节点 cur.next
            ListNode tmp = cur.next;
            // 修改 next 引用指向
            cur.next = pre;
            // pre 暂存 cur
            pre = cur;
            //cur 指向下一节点
            cur = tmp;
        }

        return pre;
    }
}
```

