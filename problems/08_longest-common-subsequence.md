### 最长公共子序列 解析(2021-07-03)

#### 题目描述

```
最长公共子序列：

给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。
一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
 1.例如，”ace” 是 “abcde” 的子序列，但 “aec” 不是 “abcde” 的子序列。
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。

示例 1：
 输入：text1 = “abcde”, text2 = “ace” 
 输出：3  
 解释：最长公共子序列是 “ace” ，它的长度为 3 。

示例 2：
 输入：text1 = “abc”, text2 = “abc”
 输出：3
 解释：最长公共子序列是 “abc” ，它的长度为 3 。

示例 3：
 输入：text1 = “abc”, text2 = “def”
 输出：0
 解释：两个字符串没有公共子序列，返回 0 。

提示：
 1. 1 <= text1.length, text2.length <= 1000
 2. Text1 和 text2 仅由小写英文字符组成。

本题来源：
 https://leetcode-cn.com/problems/longest-common-subsequence/
```

##### 思路：**使用动态规划思想**

* 假设 2 个序列分别是 nums1、nums2
* **定义状态方程**
  * 假设 dp(i, j) 是【nums1 前 i 个元素】与【nums2 前 j 个元素】的最长公共子序列长度
*  **定义初始值**
  * dp(i, 0)、dp(0, j) 初始值均为 0
*  **定义状态转移方程**
  * 假设 nums1[i – 1] = nums2[j – 1]，那么 dp(i, j) = dp(i – 1, j – 1) + 1
  * 假设 nums1[i – 1] ≠ nums2[j – 1]，那么 dp(i, j) = max { dp(i – 1, j), dp(i, j – 1) }

##### 实现方式1：使用递归

* 这种方式实现较为简单，易于理解
* 但是这种时间复杂度较高，不推荐！！！
* 时间复杂度：O(2^n) ，当 n = m 时
* 空间复杂度：O(k)，k = min{n, m}
  * n，m是两个序列的长度

###### java代码实现

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        //过滤掉不合理的值
        if(text1 == null || text2 == null){
            return 0;
        }
        //先把字符串转成数组
        char[] arr1 = text1.toCharArray();
        char[] arr2 = text2.toCharArray();

        return getlsc(arr1,arr1.length,arr2,arr2.length);
    }

    public static int getlsc(char[] arr1, int i ,char[] arr2 ,int j){
        //定义初始状态
        if(i == 0 || j == 0){
            return 0;
        }
        //定义状态方程
        if(arr1[i - 1] == arr2[j -1]){
            return getlsc(arr1, i - 1, arr2, j - 1) + 1;
        }else{
            return Math.max(getlsc(arr1, i - 1, arr2, j), getlsc(arr1, i, arr2, j - 1));
        }
    }
}
```



#####  实现方式2：使用递推

* 准备二维一个数组记录下子问题的解
* 使用递推方式自下而上计算 
* 时间复杂度：O(n ∗ m)
* 空间复杂度：O(n ∗ m)
  * n，m是两个序列的长度

###### java代码实现

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        //过滤掉不合理的值
        if(text1 == null || text2 == null){
            return 0;
        }
        //先把字符串转成数组
        char[] arr1 = text1.toCharArray();
        char[] arr2 = text2.toCharArray();

        //定义一个二维一个数组记录下子问题的解，使用递推方式自下而上计算
        int[][] dp = new int[arr1.length + 1][arr2.length + 1];
        for(int i = 1; i <= arr1.length; i++){
            for(int j = 1; j<= arr2.length; j++){
                if(arr1[i - 1] == arr2[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = Math.max(dp[i -1][j], dp[i][ j - 1]);
                }
            }
        }

        return dp[arr1.length][arr2.length];
    }

  
}
```



#####  实现方式3：使用递推（降低空间复杂度）

* 一维数组记录下子问题的解
* 使用递推方式自下而上计算 
* 通过方式2，可以看出，使用二维数组，实际取值时，只会取二维数组 i位置或 j位置 前一位的值
* 这种造成空间的浪费，所以可以采用一维数组来代替
* 一维数组来记录前面的变量时，难点在于：
  * 一维数组随着 j 的变化，前面的值会被覆盖
  * 所以要使用变量来记录被覆盖之前的值
* 时间复杂度：O(n ∗ m)
* 空间复杂度：O(n)
  * n，m是两个序列的长度

###### java代码实现

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        //过滤掉不合理的值
        if(text1 == null || text2 == null){
            return 0;
        }
        //先把字符串转成数组
        char[] arr1 = text1.toCharArray();
        char[] arr2 = text2.toCharArray();

        //定义一个一维一个数组记录下子问题的解，使用递推方式自下而上计算
        int[] dp = new int[arr2.length + 1];
        for(int i = 1; i <= arr1.length; i++){
            int tmp = 0;
            for(int j = 1; j<= arr2.length; j++){
                //判断是否相等
                int leftTop = tmp;
                tmp = dp[j];

                if(arr1[i - 1] == arr2[j - 1]){
                    dp[j] = leftTop + 1;
                }else{
                    dp[j] = Math.max(dp[j], dp[j - 1]);
                }
            }
        }

        return dp[arr2.length];
    }

  
}
```

