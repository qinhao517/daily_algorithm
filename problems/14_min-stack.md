# 最小栈解析

## 题目描述

```text
设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈
* push(x) —— 将元素 x 推入栈中。
* pop() —— 删除栈顶的元素。
* top() —— 获取栈顶元素。
* getMin() —— 检索栈中的最小元素。

示例:
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]
输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); --> 返回 -3.
minStack.pop();
minStack.top(); --> 返回 0.
minStack.getMin(); --> 返回 -2.

提示：
* pop、top 和 getMin 操作总是在 非空栈 上调用。

来源：https://leetcode-cn.com/problems/min-stack/
```

## 思路：使用两个队列实现最小栈

* 主栈，存放所有入栈数据
* 每次入栈当前最小数据

## java代码示例

```java
class MinStack { 
    Deque<Integer> xStack; //主栈，存放所有入栈数据
    Deque<Integer> minStack; //每次入栈当前最小数据
    public MinStack() {
        xStack = new LinkedList<Integer>();
        minStack = new LinkedList<Integer>();
        minStack.push(Integer.MAX_VALUE);//默认最小数据为整形最大值
    }

    public void push(int x) {
        xStack.push(x);//入栈
        minStack.push( minStack.peek()< x? minStack.peek():x); //将当前最小值入栈，为保证数量一致，所以会重复存放最小值
    }

    public void pop() {
        xStack.pop();//出栈
        minStack.pop();//当前最小值也出栈
    }

    public int top() {
        return xStack.peek();//获取栈顶元素
    }

    public int getMin() {
        return minStack.peek();//通过最小栈获取当前最小元素
    }
}

```

