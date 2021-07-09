### 最大子序和(2021-07-01) 题目解析

#### 题目描述

```
最大子序和：
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例 1：
	输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
	输出：6
	解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。

示例 2：
	输入：nums = [1]
	输出：1

提示：
	1 <= nums.length <= 3 * 104
	-105 <= nums[I] <= 105

本题来源：
	https://leetcode-cn.com/problems/maximum-subarray/
```

#### 题目解析

##### 总体思路：**使用动态规划解题**

* 定义状态
  * dp(i) 是以 nums[i] 结尾的最大连续子序列和（nums是整个序列）
* 初始状态
  * dp(0)=nums[0]
* 状态转移方程
  * 如果以 nums[0] –2 结尾，则最大连续子序列是 –2，所以 dp(0) = –2 
  * 如果以 nums[1] 1 结尾，则最大连续子序列是 1，所以 dp(1) = 1 
  * 如果以 nums[2] –3 结尾，则最大连续子序列是 1、–3，所以 dp(2) = dp(1) + (–3) = –2 
  * 如果以 nums[3] 4 结尾，则最大连续子序列是 4，所以 dp(3) = 4
  * 如果以 nums[4] –1 结尾，则最大连续子序列是 4、–1，所以 dp(4) = dp(3) + (–1) = 3 
  * 如果以 nums[5] 2 结尾，则最大连续子序列是 4、–1、2，所以 dp(5) = dp(4) + 2 = 5 
  * 以此类推。。。



###### java代码示例

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //过滤异常数据
        if(nums == null || nums.length == 0){
            return 0;
        }

        //定义数组
        int[] dp = new int[nums.length];
        //定义初始状态
        dp[0] = nums[0];

        //初始化最大子序列和
        int max = nums[0];
        //遍历数组
        for (int i = 1; i < nums.length; i++) {
            if(dp[i - 1] <= 0){
                dp[i] = nums[i];
            }else {
                dp[i] = dp[i -1] + nums[i];
            }
            max = Math.max(max,dp[i]);
        }

        return max;
    }
}
```



###### PHP代码示例

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function maxSubArray($nums) {
        //过滤异常数据
        if(empty($nums)){
            return 0;
        }

        //定义数组
        $dp = [];
        //定义初始状态
        $dp[0] = $nums[0];
        //初始化最大子序列和
        $max = $nums[0];
        //遍历数组
        for($i = 1; $i < count($nums); $i++){
            if($dp[$i - 1] <= 0){
                $dp[$i] = $nums[$i];
            }else{
                $dp[$i] = $dp[$i - 1] + $nums[$i];
            }

            $max = max($max,$dp[$i]);
        }
        return $max;
    }
}
```



##### 优化

* 使用变量替代数组
* 这种方式对于时间复杂度没有优化的地方，但是对于空间复杂度有优化：O(n)  ==> O(1)

###### java代码示例

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //过滤异常数据
        if(nums == null || nums.length == 0){
            return 0;
        }

        //定义变量
        int dp = nums[0];
       
        //初始化最大子序列和
        int max = dp;
        //遍历数组
        for (int i = 1; i < nums.length; i++) {
            if(dp <= 0){
                dp = nums[i];
            }else {
                dp = dp + nums[i];
            }
            max = Math.max(max,dp);
        }

        return max;
    }
}
```

