# 题目：剑指 Offer 29. 顺时针打印矩阵
**难度**： 简单

**题目**：

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。



## 示例 1：

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

## 示例 2：

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```



## 说明：

- `0 <= matrix.length <= 100`
- `0 <= matrix[i].length <= 100`

注意：本题与主站 54 题相同：https://leetcode-cn.com/problems/spiral-matrix/



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

- 使用按层模拟的方法，定义left、right、top、low分别指向矩阵的四个顶点，并且顶点会不断缩小。然后按顺时针的方向，不断模拟整个过程，修改i、j指针。
- 注意，在我的代码里，循环判断的情况是`left <= right && low <= top`，原因在于，当矩阵的行的个数为奇数，列(3 * 3)为奇数时，会导致`left==right，low==top`的情况，所以也需要判断。这时候，在循环里的if判断里，在特殊情况下的跳转就需要判断`left != right，low != top`。

# 解题代码




```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {

        vector<int> ans;
        int m = matrix.size();      //行
        if (m == 0)
            return ans;
        int n = matrix[0].size();   //列
        
        int left = 0, right = n - 1;
        int top = 0, low = m - 1;

        
        int i = 0, j = 0;
        ans.push_back(matrix[i][j]);
        while (left <= right && top <= low)
        {
            if (j < right && i == top)
            {
                ans.push_back(matrix[i][++j]);
            }
            else if (j == right && i < low)
            {
                ans.push_back(matrix[++i][j]);
            }
            else if (j > left && i == low && top != low)
            {
                ans.push_back(matrix[i][--j]);
            }
            else if (j == left && i > top + 1 && left != right)
            {
                ans.push_back(matrix[--i][j]);
            }
            else if (j == left && i == top + 1)
            {
                left++;
                right--;
                top++;
                low--;
            }
            else
            {
                break;
            }
            //cout << matrix[i][j] << endl;
        }

        return ans;
    }
};
```


