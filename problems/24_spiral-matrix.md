# 螺旋矩阵

## 题目描述

```
给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素

示例 1：
  输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
  输出：[1,2,3,6,9,8,7,4,5]
  
示例 2：
  输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
  输出：[1,2,3,4,8,12,11,10,9,5,6,7]
  
提示：
* m == matrix.length
* n == matrix[i].length
* 1 <= m, n <= 10
* -100 <= matrix[i][j] <= 100

链接：https://leetcode-cn.com/problems/spiral-matrix
```

## 解题思路

* 首先设定上下左右边界
* 其次向右移动到最右，此时第一行因为已经使用过了，可以将其从图中删去，体现在代码中就是重新定义上边界
* 判断若重新定义后，上下边界交错，表明螺旋矩阵遍历结束，跳出循环，返回答案
* 若上下边界不交错，则遍历还未结束，接着向下向左向上移动，操作过程与第一，二步同理
* 不断循环以上步骤，直到某两条边界交错，跳出循环，返回答案

## java代码示例

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        //定义一个list
        List<Integer> res = new LinkedList<>();
        if(matrix.length == 0){
            return res;
        }

        int top = 0;//定义上边界
        int bottom = matrix.length - 1;//定义下边界
        int right = matrix[0].length - 1;//定义右边界
        int left = 0;//定义左边界
        
        //开始遍历
        while(true){
            for(int i = left; i <= right; i++){//向右移动到最右
                res.add(matrix[top][i]);
            }
            if(++top > bottom){//如果上边界大于下边界，则遍历结束
                break;
            }

            for(int i= top; i <= bottom; i++){//向下移动到最下面
                res.add(matrix[i][right]);
            }

            if(--right < left){//如果右边界小于左边界，则遍历结束
                break;
            }

            for(int i = right; i >= left; i--){//向左移动到最左边
                res.add(matrix[bottom][i]);
            }
            if(--bottom < top){//如果下边界小于上边界，则遍历结束
                break;
            }

            for(int i = bottom; i >= top; i-- ){//向上移动到最上边
                res.add(matrix[i][left]);
            }

            if(++left > right){
                break;
            }
        }

        return res;
    }
}
```

