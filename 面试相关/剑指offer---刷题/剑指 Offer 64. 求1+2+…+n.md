# 题目：剑指 Offer 64. 求1+2+…+n

**难度**： 中等

**题目**：

求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。



## 示例 1：

```
输入: n = 3
输出: 6
```

## 示例 2：

```
输入: n = 9
输出: 45
```



## 提示：

- `1 <= n <= 10000`



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/qiu-12n-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

不能使用诸多运算符，只剩加减和逻辑运算符了，这里使用了很关键的&&。

按照递归的思路，需要`if(n == 0) return n;`，但是不可以用if，如何返回递归？可以用&&。

逻辑运算符 && 为例，对于 A && B 这个表达式，如果 A 表达式返回 \textit{False}False ，那么 A && B 已经确定为 False ，此时不会去执行表达式 B。同理，对于逻辑运算符 ||， 对于 A || B 这个表达式，如果 A 表达式返回 True ，那么 A || B 已经确定为 True ，此时不会去执行表达式 B。



# 解题代码




```cpp
class Solution {
public:
    int sumNums(int n) {

        n && (n += sumNums(n - 1));
        return n;
    }
};
```



