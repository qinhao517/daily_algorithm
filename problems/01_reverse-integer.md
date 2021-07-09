### 数字反转算法(2021-06-22) 题目解析

#### 题目描述

```
给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 [−231,  231 − 1] ，就返回 0。

假设环境不允许存储 64 位整数（有符号或无符号）

示例 1：
	输入：x = 123
	输出：321

示例 2：
	输入：x = -123
	输出：-321

示例 3：
	输入：x = 120
	输出：21	

示例 4：
	输入：x = 0
	输出：0

提示：-231 <= x <= 231 - 1

本题来源：
	Leetcode 7 https://leetcode-cn.com/problems/reverse-integer/
```



### java实现

#### 题目解析

##### 思路1：**暴力解法：逆序输出**

* 整数转字符串，再转字符数组 
* 反向遍历字符数组，并将元素存储到新数组中
* 将新数组转成字符串，再转成整数输出

###### 注意事项：

* 边界问题
  * 数组索引越界
  * 数值溢出边界：溢出则返回0
* 细节问题
  * 首位不为0
  * 符号处理 

###### java代码示例

```java
class Solution {
    public int reverse(int x) {
        //数值越界，直接返回0
        if(x == Integer.MAX_VALUE || x == Integer.MIN_VALUE){
            return 0;
        }

        //定义符号标识
        int flag = x > 0 ? 1 : -1;

        //无论正负，统一处理成正数
        x = x > 0 ? x : -x;

        //把数字变成字符串,再转成数组进行翻转操作
        String str = String.valueOf(x);
        char[] chars = str.toCharArray();

        //遍历数组，把翻转的数据存到新的数组
        int length = chars.length;
        //定义新的数组
        char[] newChars = new char[length];
        for(int i=0; i<length; i++){
            newChars[i] = chars[length - i -1];
        }

        //把新数组变成字符串
        String newStr = String.valueOf(newChars);
        //再转成数字
        long newX = Long.valueOf(newStr);

        //判断翻转后的数值是否越界
        if(newX > Integer.MAX_VALUE || newX < Integer.MIN_VALUE){
            return 0;
        }

        //返回结果
        return (int)newX * flag;

    }
}
```





##### 思路2：**优化解法：首尾交换**

* 整数转字符串，再转字符数组
* 交换首位(start)和末位(end)数字
* 循环操作：依次交换第二(start++)和倒数第二个(end--) ，直到数组剩下1个或0个元素 
* 将原数组转成字符串，再转成整数输出 

###### 注意事项：

* 边界问题
  * 数组索引越界：数组长度为偶数，反转完成标志为start>end；为奇数时反转完成标志为start==end 
* 其他要注意事项同上



###### java代码示例

```java
class Solution {
    public int reverse(int x) {
        //数值越界，直接返回0
        if(x == Integer.MAX_VALUE || x == Integer.MIN_VALUE){
            return 0;
        }

        //定义符号标识
        int flag = x > 0 ? 1 : -1;

        //无论正负，统一处理成正数
        x = x > 0 ? x : -x;

        //把数字变成字符串,再转成数组进行翻转操作
        String str = String.valueOf(x);
        char[] chars = str.toCharArray();
        int length = chars.length;

        //首尾交换，达到翻转的目的,使用双指针方法来判断循环的结束时机
        int start = 0, end = length -1;
        while(start < end){
            char tmp = chars[start];
            chars[start] = chars[end];
            chars[end] = tmp;
            
            //指针分别移动一位
            start++;
            end--;
        }
        
        //把新数组变成字符串
        String newStr = String.valueOf(chars);
        //再转成数字
        long newX = Long.valueOf(newStr);

        //判断翻转后的数值是否越界
        if(newX > Integer.MAX_VALUE || newX < Integer.MIN_VALUE){
            return 0;
        }

        //返回结果
        return (int)newX * flag;

    }
}
```

##### 思路3 (推荐解法！)：**数学思维解法**

* 尝试拿个位数字 ，对10取模运算得到个位数字
* 让每一位数字变成个位数字，先除以10，再对10取模得到十位数字 
* 循环上述操作 
* 将每一位数字计算累加，将上次累加结果*10 + 新数字

###### 注意事项：

* 边界问题：
  * 从低位到高位处理，最高位结束
  * 最高位 / 10 == 0
  * 最高位 % 10 == 最高位 
  * 数值溢出边界：溢出则返回0 
* 其他要注意事项同上

###### java代码示例

```java
class Solution {
    public int reverse(int x) {
        //数值越界，直接返回0
        if(x == Integer.MAX_VALUE || x == Integer.MIN_VALUE){
            return 0;
        }

        //定义符号标识
        int flag = x > 0 ? 1 : -1;

        //无论正负，统一处理成正数
        x = x > 0 ? x : -x;

        // 1.尝试拿个位数字：对10取模运算 
        // 2.让每一位数字变成个位数字：先除以10，再对10取模得到十位数字
        int last  = 0;//定义末位
        int result = 0; // 返回结果
        while((last = x % 10) != x){
            // 3.将每一位数字计算累加：将上次累加结果*10 + 新数字
            result = result * 10 + last;
            x /= 10;
        }

        if(last != 0){
            long re = result; 
            re = re * 10 + last; 
            if (re > Integer.MAX_VALUE || re < Integer.MIN_VALUE) { 
                result = 0; 
            } else { 
                result = (int)re; 
            }
        }

        return result * flag; // 返回前进行符号处理
    }
}
```



### PHP实现

#### 题目解析

##### 备注：

* 这里只给出对应的推荐最优解参考代码
* 思路1，2的代码可以根据上面的思路，自行实现

##### 思路3 (推荐解法！)：**数学思维解法**

* 尝试拿个位数字 ，对10取模运算得到个位数字
* 让每一位数字变成个位数字，先除以10，再对10取模得到十位数字 
* 循环上述操作 
* 将每一位数字计算累加，将上次累加结果*10 + 新数字



###### Php代码示例

```php
class Solution {

    /**
     * @param Integer $x
     * @return Integer
     */
    function reverse($x) {
        $result=0;//返回结果
        while($x != 0){
          	//尝试拿个位数字 ，对10取模运算得到个位数字
            $last = $x % 10;
          	
          	//让每一位数字变成个位数字，先除以10，再对10取模得到十位数字 
            $x = intval($x / 10);
          
          	//每一位数字计算累加，将上次累加结果*10 + 新数字
            $result = $result * 10 + $last;
        }
        if ($result > (pow(2,31)-1) || $result < pow(-2,31)) {
            return 0;
        }
        return $result;
    }
}
```



