# 题目：剑指 Offer 17. 打印从1到最大的n位数
**难度**： 简单

**题目**：
输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。



## 示例 1：

```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```



## 说明：

- 用返回一个整数列表来代替打印
- n 为正整数



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

（1）因为题目给顶返回vector<int>数组，所以不需要考虑大数问题，直接逐个加入即可。可采取pow或直接计算求最大范围。

（2）如果考虑到大数问题，即数字可能操作int最大范围，那么我们可以利用**全排列**的方式，根据n的位数不断DFS，无论多少位都可以解决。

# 解题代码

## （1）暴力法


```cpp
class Solution {
public:
    vector<int> printNumbers(int n) {

        /*
        int tmp = 1;
        while (n)
        {
            tmp *= 10;
            n -= 1;
        }
        */
        int tmp = pow(10, n);
        vector<int> arr;
        for (int i = 1; i < tmp; i++)
            arr.push_back(i);

        return arr;
    }
};
```

## （2）DFS全排列-解决大数问题

```
class Solution {
private:
    vector<int> ans;
public:
    vector<int> printNumbers(int n) {

        string s = "0123456789";
        string str = "";
        DFS(s, str, n);
        auto it = ans.begin();
        ans.erase(ans.begin());
        return ans;
        
    }
    void DFS(string& s, string str, int n)
    {
        if (str.size() == n)
        {
            //如果大数问题，不用ans存储，采取字符串存储或直接打印
            ans.push_back(atoi(str.c_str()));
            return;
        }

        for (int i = 0; i < s.size(); i++)//全排列需从0~9
        {
            str += s[i];
            DFS(s, str, n);//根据位数n判断
            str.pop_back();
        }

    }
};
```

