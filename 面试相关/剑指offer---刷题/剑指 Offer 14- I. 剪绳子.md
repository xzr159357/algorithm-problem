# 题目：剑指 Offer 14- I. 剪绳子
**难度**： 中等

**题目**：
给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。







## 示例 1：

```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```

## 示例 2：

```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

## 提示：

- 2 <= n <= 58



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jian-sheng-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
1. 看题意，首先的思路，就是枚举出所有的情况，求出其最小值，递归求解。递归必然会超时，所以这时候考虑动态规划或者其它。但是纵观这题，利用已有递归算法求解前15项，发现出规律：从第四项后开始，即n=5开始，其实就是尽可能地将n分解为多个3，多一则取4，多二则2*3。
2. 以上数学方法是通过找规律得到，并无证明。因此我们采取动态规划，设dp[i]为长度为i的绳子最大乘积，长度为n的返回dp[n]即可。动态转化方程：dp[i] = max(dp[i], dp[j] + dp[i - j])，结果的最大乘积就是dp[j]长度为j的最大乘积加上剩余i-j长度的最大乘积。

# 解题代码

# （1）数学


```cpp
class Solution {
private:
    int sum;
public:
    int cuttingRope(int n) {

        if (n <= 2)
            return 1;
        if (n == 3)
            return 2;
        if (n == 4)
            return 4;
        int res = 1;
        int tmp = n / 3;
        res = pow(3, tmp - 1);
        int num = n % 3;
        if (num == 0)
            res *= 3;
        else if (num == 1)
            res *= 4;
        else
            res *= 6;
        return res;
        /*
        sum = INT_MIN;
        for (int i = 1; i < n; i++)
        {
            DFS(n - i, i);
        }
        return sum;
        */

    }
    /*
    void DFS(int n, int res)
    {
        if (n <= 1)
        {
            sum = max(sum, res);
            return;
        }
        for (int i = 1; i <= n; i++)
        {
            DFS(n - i, res * i);
        }
    }
    */
};
```

# （2）动态规划

```cpp
class Solution {
public:
    int cuttingRope(int n) {

        if (n <= 1) return 0;
        if (n == 2) return 1;//1*1
        if (n == 3) return 2;//1*2

        vector<int> dp(n + 1, 0);//dp含义：长度为i的数组的最大乘积
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;

        for (int i = 4; i <= n; i++)
        {
            for (int j = 1; j <= i / 2; j++)    //j<i，但是另一半重复取i/2
            {
                dp[i] = max(dp[i], dp[j] * dp[i - j]);//判断i/2次找出最大值
            }
        }
        return dp[n];        

    }
    
};
```

