### 有效数字 算法题解析(2021-06-29 )

#### 题目描述

```
有效数字

验证给定的字符串是否可以解释为十进制数字。

有效数字（按顺序）可以分成以下几个部分：
	1. 一个 小数 或者 整数
	2.（可选）一个 'e' 或 'E' ，后面跟着一个 整数

小数（按顺序）可以分成以下几个部分：
	1.（可选）一个符号字符（'+' 或 '-'）
	2.下述格式之一：
		1.至少一位数字，后面跟着一个点 '.'
		2.至少一位数字，后面跟着一个点 '.' ，后面再跟着至少一位数字
		3.一个点 '.' ，后面跟着至少一位数字

整数（按顺序）可以分成以下几个部分：
	1.（可选）一个符号字符（'+' 或 '-'）
	2.至少一位数字

部分有效数字列举如下：
	["2", "0089", "-0.1", "+3.14", "4.", "-.9", "2e10", "-90E3", "3e+7", "+6e-1", "53.5e93", "-123.456e789"]

部分无效数字列举如下：
	["abc", "1a", "1e", "e3", "99e2.5", "--6", "-+3", "95a54e53"]

给你一个字符串 s ，如果 s 是一个 有效数字 ，请返回 true 。


示例 1：
	输入：s = "0"
	输出：true

示例 2：
	输入：s = "e"
	输出：false

示例 3：
	输入：s = "."
	输出：false

示例 4：
	输入：s = ".1"
	输出：true

提示：
	1. 1 <= s.length <= 20
	2. s仅含英文字母（大写和小写），数字（0-9），加号 '+' ，减号 '-' ，或者点 '.' 。

本题来源：
	https://leetcode-cn.com/problems/valid-number/
```

##### 思路1：**暴⼒解法**

* 设定numBool，dotBool和eBool三种boolean变量，分别代表是否出现数字、点和E
* 判断是否属于数字的0~9区间
* 遇到点的时候，判断前面是否有点或者E，都需要return false
* 遇到E的时候，判断前面数字是否合理，是否有E，并把numBool置为false，防止E后无数字
* 遇到-+的时候，判断是否是第一个，如果不是第一个判断是否在E后面，都不满足则return false
* 其他情况都为false
* 最后根据numBool的结果判断即可

###### java代码示例

```java
class Solution {
    public boolean isNumber(String s) {
        if (s == null || s.length() == 0) return false;
        boolean numBool = false;// 表示是否出现数字
        boolean dotBool = false;// 表示是否出现点
        boolean eBool = false;// 表示是否出现E

        //字符串转成数组
        char arr[] = s.trim().toCharArray();

        //遍历数组
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] >= '0' && arr[i] <= '9') {
                numBool = true;
            } else if (arr[i] == '.') {
                if (dotBool || eBool) {
                    return false;
                }
                dotBool = true;
            } else if (arr[i] == 'E' || arr[i] == 'e') {
                if (eBool || !numBool) {
                    return false;
                }
                eBool = true;
                numBool = false;
            } else if (arr[i] == '+' || arr[i] == '-') {
                if (i != 0 && arr[i - 1] != 'e' && arr[i - 1] != 'E') {
                    return false;
                }
            } else {
                return false;
            }
        }
        return numBool;
    }
}
```

###### PHP代码示例

```php
class Solution {

    /**
     * @param String $s
     * @return Boolean
     */
    function isNumber($s) {
        if ($s == null || strlen($s) == 0) return false;
        $numBool = false;// 表示是否出现数字
        $dotBool = false;// 表示是否出现点
        $eBool = false;// 表示是否出现E

        //遍历字符串
        for($i = 0; $i < strlen($s); $i++){
            $a = $s[$i];
            if(is_numeric($a)){
                $numBool = true;
            }else if($s[$i] == "."){
                if($dotBool || $eBool){
                    return false;
                }
                $dotBool = true;
            }else if($s[$i] == "e" || $s[$i] == "E"){
                if(!$numBool || $eBool){
                    return false;
                }
                $eBool = true;
                $numBool = false;
            }else if($s[$i] == "+" || $s[$i] == "-"){
                if($i != 0 && $s[$i-1] != "e" && $s[$i-1] != "E"){
                    return false;
                }
            }else{
                return false;
            }
        }

        return $numBool;
    }
}
```



