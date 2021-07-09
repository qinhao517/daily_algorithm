# 合并两个有序链表(2021-07-07)

## 题目描述

```text
合并两个有序链表:
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

示例 1：
	输入：l1 = [1,2,4], l2 = [1,3,4]
	输出：[1,1,2,3,4,4]

示例 2：
	输入：l1 = [], l2 = []
	输出：[]

示例 3：
	输入：l1 = [], l2 = [0]
	输出：[0]

提示：
	两个链表的节点数目范围是 [0, 50]
	-100 <= Node.val <= 100
	l1 和 l2 均按 非递减顺序 排列

本题来源：
	https://leetcode-cn.com/problems/merge-two-sorted-lists/submissions/
```

## 思路1：暴力解法

* 直接遍历两个链表，取出链表中每个元素，放到一个list中
* 对list进行排序
* 对排序后的list进行迭代，生成新的链表

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

    /**
    * 暴力解法，把两个链表的所有val取出来放到一个list中，然后进行排序，最新生成新的链表
     */
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }
        //定义list
        List<Integer> list = new ArrayList<>();

        //遍历l1
        while(l1 != null){
            list.add(l1.val);
            l1 = l1.next;
        }

        //遍历l2
        while(l2 != null){
            list.add(l2.val);
             l2 = l2.next;
        }

        //升序排列
        Collections.sort(list);
        //生成新的链表
        ListNode newList = new ListNode(-1);
        //定义结果链表的遍历指针，代表当前操作的节点
        ListNode cur = newList;
        //增强的for循环
        for(int n : list){
            //将当前的数值插⼊新链表尾部
            cur.next = new ListNode(n);
            //指针节点重新指向新的节点
            cur = cur.next;
        }

        return newList.next;
    }
}
```

## 思路2：利用哨兵节点

* 定义一个哨兵节点，也可以理解其为中间变量，只不过这个变量是链表
* 利用哨兵节点来存放合并后的链表
* 最后判断遍历结束之后，两个链表是否为null,有不为null的，把哨兵节点的指针指向该链表即可



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
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }

        //利用哨兵结点机制来处理
        ListNode soldier = new ListNode(-1);
        ListNode p = soldier;
        
        //遍历链表，进行每个节点值域的比较
        while(l1 != null && l2 != null){
            if(l1.val < l2.val){
                p.next = l1;
                l1 = l1.next;
            }else{
                p.next = l2;
                l2 = l2.next;
            }
            p = p.next;
        }

      	//判断遍历结束之后，两个链表是否为null,有不为null的，把哨兵节点的指针指向该链表即可
        if(l1 != null){
            p.next = l1;
        }

        if(l2 != null){
            p.next = l2;
        }

        return soldier.next;

    }
}
```

