# 题目：剑指 Offer 04. 二维数组中的查找
**难度**： 中等	

**题目**：
在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。



## 示例1

现有矩阵 matrix 如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定 target = `5`，返回 `true`。

给定 target = `20`，返回 `false`。



## 限制：

- 0 <= n <= 1000
- 0 <= m <= 1000



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
（1）暴力法，直接遍历二维数组求解。

（2）利用其从左往右变大，从上往下变大的性质，那么在**右上角**必然是：该元素**小于下边大于左边**，因此该元素如果不等于target就只能**往左**（大于target） or 往下（小于target）。因此就固定了位置，双指针即可解决。同理，类似的二维排序题目，观察出只能有两种路径的走法就可解题。**比如**，从上往下递减，从左往右递减的二维数组，采取左下角。



# 解题代码
## （1）暴力法
```cpp
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {

        
        for (int i = 0; i < matrix.size(); i++)
        {
            for (int j = 0; j < matrix[0].size(); j++)
            {
                if (matrix[i][j] == target)
                    return true;
            }
        }
        return false;
        

    }
};
```



## （2）优化法

```cpp
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {

  
        //开始点选取右上角，往下比其大，往左比其小。该特点可解该类问题
        int n = matrix.size();
        if (n == 0)
            return false;
        int m = matrix[0].size();
        if (m == 0)
            return false;

        int i = 0, j = m - 1;
        while (i < n && j >= 0)
        {
            if (matrix[i][j] == target)
            {
                return true;
            }
            else if(matrix[i][j] > target)
            {
                j--;
            }
            else
            {
                i++;
            }
        }
        return false;
        
    }
};
```


