# 题目：剑指 Offer 19. 正则表达式匹配
**难度**： 困难

**题目**：

请实现一个函数用来匹配包含'. '和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab*a"均不匹配。





## 示例 1：

```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```

## 示例 2：

```
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

## 示例 3：

```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```

## 示例 4：

```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```

## 示例 5：

```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```



## 说明：

- `s` 可能为空，且只包含从 `a-z` 的小写字母。
- `p` 可能为空，且只包含从 `a-z` 的小写字母以及字符 `.` 和 `*`，无连续的 `'*'`。



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

- 正则表达式的匹配，比较复杂，需要用到动态规划。
- ![image-20210509102306988](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210509102306988.png)

# 解题代码


```cpp
class Solution {
public:
    bool isMatch(string s, string p) {

        int m = s.size();
        int n = p.size();

        //lamda表达式，该函数用于是否判断s[i-1]和p[j-1]是否相等
        auto matches = [&](int i, int j)
        {
            if (i == 0)
                return false;
            
            //如果是'.'，那么这次不用匹配
            if (p[j - 1] == '.')
                return true;

            return s[i - 1] == p[j - 1];
        };

        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        dp[0][0] = true;
        for (int i = 0; i <= m; i++)
        {
            for (int j = 1; j <= n; j++)
            {
                if (p[j - 1] == '*')//判断*
                {
                    dp[i][j] |= dp[i][j - 2];//*之前的元素
                    if (matches(i, j - 1))//匹配*之前的元素，所以用j-1，而matches里面又j-1，即j-2
                    {
                        dp[i][j] |= dp[i - 1][j];
                    }
                }
                else
                {
                    if (matches(i, j))
                    {
                        dp[i][j] |= dp[i - 1][j - 1];
                    }
                }
            }
        }

        return dp[m][n];
    }
};
```