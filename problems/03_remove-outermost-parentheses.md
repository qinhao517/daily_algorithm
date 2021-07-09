### 删除最外层的括号 算法题解析(2021-06-24 )

#### 题目描述

```
有效括号字符串为空 ("")、"(" + A + ")" 或 A + B，其中 A 和 B 都是有效的括号字符串，+ 代表字符串的连接。例如，""，"()"，"(())()" 和 "(()(()))" 都是有效的括号字符串。

如果有效字符串 S 非空，且不存在将其拆分为 S = A+B 的方法，我们称其为原语（primitive），其中 A 和 B 都是非空有效括号字符串。

给出一个非空有效字符串 S，考虑将其进行原语化分解，使得：S = P_1 + P_2 + ... + P_k，其中 P_i 是有效括号字符串原语。

对 S 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 S 

示例 1：
输入："(()())(())"
输出："()()()"
解释：
输入字符串为 "(()())(())"，原语化分解得到 "(()())" + "(())"，
删除每个部分中的最外层括号后得到 "()()" + "()" = "()()()"。

示例 2：
输入："(()())(())(()(()))"
输出："()()()()(())"
解释：
输入字符串为 "(()())(())(()(()))"，原语化分解得到 "(()())" + "(())" + "(()(()))"，
删除每个部分中的最外层括号后得到 "()()" + "()" + "()(())" = "()()()()(())"。


本题来源：
https://leetcode-cn.com/problems/remove-outermost-parentheses/
```



##### 思路1：**暴⼒解法**

* 定义容器存储原语子串:new ArrayList<String>();
* 定义左括号、右括号计数器：int left = 0, right = 0; 
* 遍历字符串，读取到括号时对应计数器自增 
* 检查是否到达原语结尾，截取原语子串并添加到容器中 
* 遍历容器，删除最外层括号后合并成新串 

###### 注意事项：

* 边界问题
  * 遍历字符串，注意索引越界：i < s.length() 
  * 截取原语字符串时，注意起止索引：[start, end) 
* 细节问题
  * 需要记录上一次截取原语子串之后的位置
  * 删除原语子串的最外层括号，其实就是重新截取



###### java代码示例

```java
class Solution {
    public String removeOuterParentheses(String s) {
        // 1.定义容器存储原语子串
        List<String> list = new ArrayList<>();

        // 2.定义左括号、右括号计数器
        int left = 0, right = 0,lastOpr = 0;

        // 3.遍历字符串，读取到括号时对应计数器自增
        int len = s.length();
        for(int i = 0; i < len; i++){
            //获取单个字符
            char str = s.charAt(i);
            if(str == '('){
                left++;
            }else if(str == ')'){
                right++;
            }

            //根据计数器来判断，相等表示遇到了有效的括号字符串，例如："()"
            if(left == right){
                list.add(s.substring(lastOpr,i+1));
                lastOpr = i + 1;
            }
        }

        //遍历list容器，把最外层的括号去除
        StringBuilder sb = new StringBuilder();
        for(String str1 : list){
            sb.append(str1.substring(1, str1.length() - 1));
        }

        return sb.toString();
    }
}
```



##### 思路2：优化解法

* 定义容器存储删除外层括号后的原语子串
* 定义左括号、右括号计数器：int left = 0, right = 0; 
* 遍历字符串，读取到括号时对应计数器自增 
* 检查是否到达原语结尾，截取不包含最外层的原语子串并拼接到容器中 

###### 注意事项：

* 边界及细节同上



###### java代码示例

```java
class Solution {
    public String removeOuterParentheses(String s) {
        int len = s.length(); 
        // 1.定义容器存储删除外层括号后的原语子串 
        StringBuilder sb = new StringBuilder(); 
        // 2.定义左括号、右括号计数器 
        int left = 0, right = 0, lastOpr = 0; 
        // 3.遍历字符串，读取到括号时对应计数器自增 
        for (int i = 0; i < len; i++) { 
            char c = s.charAt(i); 
            if (c == '(') { 
                left++; 
            } else if (c == ')') { 
                right++; 
            }
            // 4.检查是否到达某个原语结尾，截取不包含最外层的原语子串添加到容器 
            if (left == right) { 
                sb.append(s.substring(++lastOpr, i)); lastOpr = i + 1; } 
            }
            
            return sb.toString();
    }
}
```



##### 思路3：最优解：栈解法

* 使用数组模拟一个栈，临时存储左括号字符
* 遍历字符串，根据情况进行入栈/出栈操作
* 读取到左括号，左括号入栈, 读取到右括号，左括号出栈
* 判断栈是否为空，若为空，找到了一个完整的原语 
* 截取不含最外层括号的原语子串并进行拼接 

###### 注意事项：

* 边界及细节同上

###### java代码示例

```java
class Solution {
    public String removeOuterParentheses(String s) {
        StringBuilder result = new StringBuilder();
        // 1.使用数组模拟一个栈，临时存储字符，替代计数器
        Stack stack = new Stack();
        int lastOpr = 0; 
        // 2.遍历字符串，根据情况进行入栈 / 出栈操作
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(c == '('){//遇到'(',此时入栈
                stack.push(c);
            }else if(c == ')'){//遇到')'，此时出栈
                stack.pop();
            }
            // 3.判断栈是否为空，若为空，找到了一个完整的原语
            if(stack.isEmpty()){
                //去掉最外层的括号，并把结果追加到resut中
                result.append(s.substring(lastOpr + 1, i));
                // 往后找，再次初始化原语开始位置
                lastOpr = i + 1;
            }
        }

        return result.toString();

    }

    /**
* 栈：后进先出
 */
class Stack<E>{
    Object[] elements = new Object[10000];
    // 栈顶索引
    int index = -1;
    // 栈中的元素个数
    int size = 0;

    public Stack(){}

    /**
    * 往栈顶插入元素
     */
    public void push(E c){
         elements[++index] = c;
         //个数加一
         size++;
    }

     /**
     * 从栈顶获取数据，不移出
      */
    public  E peek(){
        //元素为0，返回null
        if(index < 0){
            return null;
        }

        return (E)elements[index];
    }

    /**
    * 从栈顶移出元素
    */
    public E pop(){
        E e = peek();
        if(e != null){
            elements[index] = null;
            index--;
            size--;
        }
        return e;
    }

    /**
    * 查看是否为空
     */
    public boolean isEmpty(){
        return size == 0;
    }
}
}

```



###### php代码示例

```php
class Solution {

    /**
     * @param String $s
     * @return String
     */
    function removeOuterParentheses($s) {
        // 1.定义一个空数据，模拟栈操作
        $stack = [];
        $result = "";//定义最终输出的结果字符串
        // 2.遍历字符串，根据情况进行入栈 / 出栈操作

        //定义一个计数器，来记录要截取的字符串的长度
        $len = 0;
        //定义字符串截取起始位标识
      	$lastOpr = 0;
        for($i = 0; $i < strlen($s);$i++){
            $c = $s[$i];
            if($c == '('){//遇到'(',此时入栈
                array_push($stack,$c);
                $len++;
            }else if($c == ')'){//遇到')'，此时出栈
                array_pop($stack);
                $len++;
            }

            // 3.判断栈是否为空，若为空，找到了一个完整的原语
            if(empty($stack)){
                //去掉最外层的括号，并把结果追加到resut中
                $result .= substr($s, $lastOpr + 1, $len - 2);
                $lastOpr = $i + 1;
                $len = 0;//重新计数
            }
        }

        return $result;
    }
}
```

