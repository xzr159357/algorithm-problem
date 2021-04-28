# 题目：剑指 Offer 05. 替换空格
**难度**： 简单

**题目**：
请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

## 示例1

```
输入：s = "We are happy."
输出："We%20are%20happy."
```



## 限制：

- 0 <= s 的长度 <= 10000

  



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
（1）直接法，重新定义一个string， 遇到空格加上"%20"即可。

（2）如果需要空间复杂度为O(1)，那么首先计算出空格的个数，之后修改s的字符串大小（resize），而后定义双指针，从后往前匹配**原地**修改字符串。

# 解题代码
## （1）暴力法
```cpp
class Solution {
public:
    string replaceSpace(string s) {

        string str;
        int n = s.size();

        for(int i = 0; i < n; i++)
        {
            if(s[i] == ' ')
                str += "%20";
            else
                str += s[i];
        }
        return str;
        

    }
};
```



## （2）原地算法

```cpp
class Solution {
public:
    string replaceSpace(string s) {

        //原地算法
        int n = s.size();
        //计算空格
        int blank = 0;
        for (auto num : s)
        {
            if (num == ' ')
                blank++;
        }
        s.resize(n + blank * 2);//调整长度

        //空格变换为%20
        int j = n + blank * 2 - 1;
        for (int i = n - 1; i >= 0; i--)
        {
            if(s[i] == ' ')
            {
                s[j--] = '0';
                s[j--] = '2';
                s[j--] = '%';
            }
            else
            {
                s[j--] = s[i];
            }
        }
        return s;

    }
};
```

