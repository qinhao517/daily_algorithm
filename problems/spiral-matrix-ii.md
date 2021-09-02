# 螺旋矩阵 II

## 题目描述

```text
给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。
示例 1：
  输入：n = 3
  输出：[[1,2,3],[8,9,4],[7,6,5]]
  
示例 2：
  输入：n = 1
  输出：[[1]]
  
提示:
* 1 <= n <= 20

题目来源：https://leetcode-cn.com/problems/spiral-matrix-ii/
```

## 解题思路

* 首先设定上下左右边界
* 其次向右移动到最右，此时第一行因为已经使用过了，可以将其从图中删去，体现在代码中就是重新定义上边界，同时插入数组中
* 判断若重新定义后，上下边界交错，表明螺旋矩阵遍历结束，跳出循环，返回答案
* 若上下边界不交错，则遍历还未结束，接着向下向左向上移动，操作过程与第一，二步同理
* 不断循环以上步骤，直到某两条边界交错，跳出循环，返回答案



## java代码示例

```java
class Solution {
    public int[][] generateMatrix(int n) {
        //定义一个数组
        int[][] arr = new int[n][n];

        //定义标识
        int top = 0;//定义上边界
        int bottom = n - 1;//定义下边界
        int right = n - 1;//定义右边界
        int left = 0;//定义左边界
        int num = 0;
        while(true){
            //向右移动
            for(int i = left; i <= right; i++){
                num += 1;
                arr[top][i] = num;
                
            }
            if(++top > bottom){
                break;
            }
            //向下移动
            for(int i = top; i <= bottom; i++){
                num += 1;
                arr[i][right] = num;
            }
            if(--right < left){
                break;
            }

            //向左移动
            for(int i = right; i >= left; i--){
                num += 1;
                arr[bottom][i] = num ;
            }
            if(--bottom < top){
                break;
            }

            //向上移动
            for(int i = bottom; i >= top; i--){
                num += 1;
                arr[i][left] = num;
            }
            if(++left > right){
                break;
            }
        }

        return arr;


    }
}
```

