### 整数的各位积和之差 算法题解析(2021-06-28 )

#### 题目描述

```java
给你一个整数 n，请你帮忙计算并返回该整数「各位数字之积」与「各位数字之和」的差。 

示例 1：
输入：n = 234
输出：15 
解释：
各位数之积 = 2 * 3 * 4 = 24 
各位数之和 = 2 + 3 + 4 = 9 
结果 = 24 - 9 = 15

示例 2：
输入：n = 4421
输出：21
解释： 
各位数之积 = 4 * 4 * 2 * 1 = 32 
各位数之和 = 4 + 4 + 2 + 1 = 11 
结果 = 32 - 11 = 21

提示：
1 <= n <= 10^5

本题来源：
https://leetcode-cn.com/problems/subtract-the-product-and-sum-of-digits-of-an-integer
```

##### 思路：**数学思路解法**

* 循环遍历拆分后的整数

* ###### 根据求余与整除的结果，拼接结果

###### java代码示例

```
class Solution {
    public int subtractProductAndSum(int n) {
        int sum = 0;//各位数之和
        int product=1;//位数之积
        while(n != 0){
            //获取最末位的数字
            int lastOpr = n % 10;

            //求和
            sum += lastOpr;
            //求积
            product *= lastOpr;

            //重置整数
            n /= 10;
        }
        return product - sum;
    }
}
```



###### php代码示例

```php
class Solution {

    /**
     * @param Integer $n
     * @return Integer
     */
    function subtractProductAndSum($n) {
        //定义和
        $sum = 0;
        //定义积
        $product = 1;
        while($n != 0){
            //获取最末位的数字
            $lastOpr = $n % 10;

            //求和
            $sum += $lastOpr;
            //求积
            $product *= $lastOpr;

            //重置整数
            $n = floor($n / 10);
        }
        return $product - $sum;
    }
}
```

