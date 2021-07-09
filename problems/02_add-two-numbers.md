### 两数相加 算法题解析(2021-06-23 )

#### 题目描述

```
给出两个非空链表用来表示两个非负整数，
其中它们各自的位数是按照逆序的方式存储的，并且它们的每个节点只能存储一位数字
如果我们将这两个数相加起来，则会返回一个新的链表来表示它们的和
您可以假设除了数字 0 之外，这两个数都不会以 0 开头

示例：
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

```

##### 思路1：**暴⼒解法**

* 遍历两个链表使⽤数学思维分别将他们转成整数
* 对两个整数进⾏求和得到sum
* 将sum按照数学思维再转成链表

###### 注意事项：

* 若超过语⾔⽀持的数据类型范围，则报错
* 解决办法：BigInteger



###### java代码示例

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        //遍历l1，取出对应的数字
        long l1Value = 0;   
        int digit = 0;
        while(l1 != null){
            //当前位置对应的单位
            int pow = (int)Math.pow(10, digit);
            l1Value += (long)l1.val * pow;
            //移动加1
            digit++;
            //链表指向下一个节点
            l1 = l1.next;
        }

        //遍历l2，取出对应的数字
        long l2Value = 0;   
        int digit2 = 0;
        while(l2 != null){
            //当前位置对应的单位
            int pow = (int)Math.pow(10,digit2);
            l2Value += (long)l2.val * pow;
            //移动加1
            digit2++;
            //链表指向下一个节点
            l2 = l2.next;
        }

        //创建⼀个新链表，头部为空节点
        ListNode head = new ListNode();
        //定义一个节点，指向头节点
        ListNode cur = head;
        //数字相加
        long sum = l1Value + l2Value;
        //为空的判断
        if(sum == 0){
            head = new ListNode(0);
            return head;
        }

        //数字最后再转成链表
        while(sum > 0){
            //获取当前最低位
            int last = (int)sum % 10;
            //创建新的节点
            ListNode node = new ListNode(last);

            //插⼊链表尾部
            cur.next = node;

            //移除当前最低位
            sum = sum / 10;

            //链表尾部指针移动
            cur = cur.next;
            
        }
        return head.next;
    }

}
```

##### 思路2：数学思维解法

* 遍历两个链表
* 对应位置的节点数值相加
* 将计算结果插⼊新链表尾部，⼤于10，则进位，将进位加到下个节点

###### 注意事项：

* 边界问题
  * 两个链表边界：next==null
*  细节问题
  * 两个链表⻓度不⼀致，短链表⾼位视为0
  * 链表最⾼位发⽣进位，结果链表需要增加⼀个节点存放进位数字



###### java代码示例

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        //原链表的两个遍历指针
        ListNode p = l1, q = l2;

        //初始化结果链表，结果链表的头节点
        ListNode resultHead = new ListNode(-1);
        //定义结果链表的遍历指针，代表当前操作的节点
        ListNode cur = resultHead;

        //进位
        int carry = 0;
        // 1.遍历两个链表
        while(p != null || q != null){
            // 以⻓链表为准
            // 获取当前节点的值：链表较短，已⽆节点，取0
            int x = p != null ? p.val : 0;
            int y = q != null ? q.val : 0;

            // 2.对应位置的节点数值相加
            int sum = x + y + carry;
            //获取的进位的数值
            carry = sum / 10;
            //获取新位置节点的数值
            int num = sum % 10;

            // 3.将计算结果插⼊新链表尾部
            cur.next = new ListNode(num);

            //指针节点重新指向新的节点
            cur = cur.next;

            //终止条件
            p = p == null ? p : p.next;
            q = q == null ? q : q.next;
        }

         // 处理最后两个数字相加结果的进位节点
         if(carry > 0){
             cur.next = new ListNode(carry);
         }

        //返回结果
        return resultHead.next;
    }
}
```

###### PHP代码示例

```php
/**
 * 定义链表节点的数据结构
 * class ListNode {
 *     public $val = 0;
 *     public $next = null;
 *     function __construct($val = 0, $next = null) {
 *         $this->val = $val;
 *         $this->next = $next;
 *     }
 * }
 */
class Solution {

    /**
     * @param ListNode $l1
     * @param ListNode $l2
     * @return ListNode
     */
    function addTwoNumbers($l1, $l2) {
        //原链表的两个遍历指针
        $p = $l1;
        $q = $l2;

        //初始化结果链表，结果链表的头节点
        $resultHead = new ListNode(-1);

        //定义结果链表的遍历指针，代表当前操作的节点
        $cur = $resultHead;

        //进位
        $carray = 0;
        // 1.遍历两个链表
        while ($p != null || $q != null){
            // 以⻓链表为准
            // 获取当前节点的值：链表较短，已⽆节点，取0
            $x = $p != null ? $p->val : 0;
            $y = $q != null ? $q->val : 0;

            // 2.对应位置的节点数值相加
            $sum = $x + $y + $carray;
            //获取的进位的数值
            $carray = floor($sum / 10);
            //获取新位置节点的数值
            $num = $sum % 10;

            // 3.将计算结果插⼊新链表尾部
            $cur->next = new ListNode($num);

            //指针节点重新指向新的节点
            $cur = $cur->next;

            //终止条件的重新赋值
            $p = $p == null ? $p : $p->next;
            $q = $q == null ? $q : $q->next;
        }

        //这里存在一个问题，就是遍历到最后一位时，求得的进位值大于0，此时需要单独处理
        if($carray > 0){
            $cur->next = new ListNode($carray);
        }

        return $resultHead->next;
    }
}
```

