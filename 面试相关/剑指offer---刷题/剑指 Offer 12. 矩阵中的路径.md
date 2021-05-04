# 题目：剑指 Offer 12. 矩阵中的路径
**难度**： 中等

**题目**：
给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

例如，在下面的 3×4 的矩阵中包含单词 "ABCCED"（单词中的字母已标出）。

![image-20210504144834779](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210504144834779.png)





## 示例 1：

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

## 示例 2：

```
输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```



## 提示：

- `1 <= board.length <= 200`
- `1 <= board[i].length <= 200`
- `board` 和 `word` 仅由大小写英文字母组成





来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
- 递归整个矩阵，判断矩阵元素是否等于word，匹配则往二维四个方向扩展，设定一个二维数组dp标志是否被访问过，达到剪枝的效果。
- 优化，可用利用原给定数组board作为是否访问的标志。

# 解题代码

## （1）DFS


```cpp
class Solution {
private:
    vector<vector<bool>> arr;
public:
    bool exist(vector<vector<char>>& board, string word) {

        
        int k = 0;
        int n = board.size();
        int m = board[0].size();
        
        arr.resize(n, vector<bool>(m));
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if (board[i][j] == word[k])
                {
                    arr[i][j] = true;
                    if (k == word.size() - 1 || DFS(board, word, i, j, k + 1))
                        return true;
                }
                
            }
        }
        return false;
    }

    bool DFS(vector<vector<char>>& board, string word, int i, int j, int index)
    {
        
        int n = board.size();
        int m = board[0].size();
        int size = word.size();

        if (i - 1 >= 0 && !arr[i - 1][j] && board[i - 1][j] == word[index])
        {
            arr[i - 1][j] = true;
            if (index == size - 1 || DFS(board, word, i - 1, j, index + 1))
                return true;
        }
        if (i + 1 < n && !arr[i + 1][j] && board[i + 1][j] == word[index])
        {
            arr[i + 1][j] = true;
            if (index == size - 1 || DFS(board, word, i + 1, j, index + 1))
                return true;     
        }
        if (j - 1 >= 0 && !arr[i][j - 1] && board[i][j - 1] == word[index])
        {
            arr[i][j - 1] = true;
            if (index == size - 1 || DFS(board, word, i, j - 1, index + 1))
                return true;
        }
        if (j + 1 < m && !arr[i][j + 1] && board[i][j + 1] == word[index])
        {
            arr[i][j + 1] = true;
            if (index == size - 1 || DFS(board, word, i, j + 1, index + 1))
                return true;
        }
        arr[i][j] = false;
        return false;
                 
    }
};
```

## （2）简化写法

```cpp
class Solution {
private:
    int n;
    int m;
public:
    bool exist(vector<vector<char>>& board, string word) {

        n = board.size();
        m = board[0].size();

        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if(DFS(board, word, i, j, 0))
                    return true;
            }
        }
        return false;
    }

    bool DFS(vector<vector<char>>& board, string word, int i, int j, int index)
    {
        //边界判断
        if (i < 0 || i >= n || j < 0 || j >= m || board[i][j] != word[index])   return false;
        //找到成功
        if (index == word.size() - 1)    return true;
        board[i][j] = '\0';//标记为访问过

        bool res = DFS(board, word, i - 1, j, index + 1) || DFS(board, word, i + 1, j, index + 1)
                    || DFS(board, word, i, j - 1, index + 1) || DFS(board, word, i, j + 1, index + 1);
        //还原
        board[i][j] = word[index];
        return res;
    }
};
```

