# 题目：剑指 Offer 13. 机器人的运动范围
**难度**： 中等

**题目**：
地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？





## 示例 1：

```
输入：m = 2, n = 3, k = 1
输出：3
```

## 示例 2：

```
输入：m = 3, n = 1, k = 0
输出：1
```

## 提示：

- 1 <= n,m <= 100
- 0 <= k <= 20



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
- 该题依旧是二维数组的问题，解题思路其实和**剑指offer 12.矩阵中的路径**类似，通过DFS搜索下边和右边的数组元素，满足要求则sum++。

# 解题代码


```cpp
class Solution {
private:
    int sum;
public:
    int movingCount(int m, int n, int k) {

        vector<vector<bool>> dp(m, vector<bool>(n));
        sum = 0;
        DFS(dp, 0, 0, m, n, k);
        return sum;
    }

    void DFS(vector<vector<bool>>& dp, int i, int j, int m, int n, int k)
    {
        //边界情况判断
        if (i < 0 || i >= m || j < 0 || j >= n || dp[i][j])    return;
        dp[i][j] = true;
        if (countNum(i, j) <= k)
            sum++; 
        else
            return;
        
        //只需要往下或往左搜索即可，因为从右上角开始
        //DFS(dp, i - 1, j, m, n, k);
        DFS(dp, i + 1, j, m, n, k);
        //DFS(dp, i, j - 1, m, n, k);
        DFS(dp, i, j + 1, m, n, k);
    }
    //计算两数位之和
    int countNum(int x, int y)
    {
        int res = 0;
        while (x)
        {
            res += x % 10;
            x /= 10;
        }
        while (y)
        {
            res += y % 10;
            y /= 10;
        }
        return res;
    }
};
```

