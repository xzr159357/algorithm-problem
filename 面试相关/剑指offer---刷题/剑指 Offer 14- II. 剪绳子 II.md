# 题目：剑指 Offer 14- II. 剪绳子 II
**难度**： 中等

**题目**：
给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m - 1] 。请问 k[0]*k[1]*...*k[m - 1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 





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

- `2 <= n <= 1000`



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
- 数学法思路和前一题一致，但是要处理溢出问题。定义res为long即可解决问题。
- 然后由于要解决溢出问题，动态规划暂时无想法。

# 解题代码

# （1）数学


```cpp
class Solution {
public:
    int cuttingRope(int n) {

        //边界判断
        if (n <= 1) return 0;
        if (n == 2) return 1;
        if (n == 3) return 2;


        long res = 1;
        
        while (n > 4)
        {
            res = res * 3 % 1000000007;
            n -= 3;
        }
        return res * n % 1000000007;
    
    }
};
```

