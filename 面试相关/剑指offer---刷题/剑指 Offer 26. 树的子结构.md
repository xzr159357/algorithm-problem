# 题目：剑指 Offer 26. 树的子结构
**难度**： 中等

**题目**：

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

```
例如:
给定的树 A:

     3
    / \

   4   5
  / \
 1   2

给定的树 B：

   4 
  /
 1
 
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
```



## 示例 1：

```
输入：A = [1,2,3], B = [3,1]
输出：false
```

## 示例 2：

```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```

## 说明：

- `0 <= 节点个数 <= 10000`



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

- 在一颗二叉树中寻找子结构，其实还是利用二叉树的遍历方式求解。

- 其实判断是否为子结构，需要逐个对比val值，并对空指针做出判断，搞懂了其中的关系就不难了。

# 解题代码

## （1）先序遍历 + 双队列

首先，利用递归实现对于二叉树的先序遍历，然后再先序遍历的访问函数中，通过双队列判断是否是子结构。


```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    bool yeFlag;
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        
        if (!B)
            return false;

        yeFlag = false;
        DFS(A, B);
        return yeFlag;
    }

    void DFS(TreeNode* A, TreeNode* B)
    {
        if (!A)
            return;

        if (visit(A, B))
        {
            yeFlag = true;
            return;
        }
        DFS(A->left, B);
        DFS(A->right, B);
    }
    bool visit(TreeNode* A, TreeNode* B)
    {
        if (!A)
        {
            return false;
        }
        if (A->val != B->val)
        {
            return false;
        }
        
        queue<TreeNode*> queA;
        queue<TreeNode*> queB;
        queA.push(A);
        queB.push(B);
        bool flag = true;
        while (!queB.empty() && !queB.empty())
        {
            
            TreeNode* tmpA = queA.front();
            queA.pop();
            TreeNode* tmpB = queB.front();
            queB.pop();

            if (tmpB->left)//B树左子树不为空
            {
                if (tmpA->left && tmpA->left->val == tmpB->left->val)
                {
                    queA.push(tmpA->left);
                    queB.push(tmpB->left);
                }
                else
                {
                    flag = false;
                    break;
                }
            }

            if (tmpB->right)//B树右子树不为空
            {
                if (tmpA->right && tmpA->right->val == tmpB->right->val)
                {
                    queA.push(tmpA->right);
                    queB.push(tmpB->right);
                }
                else
                {
                    flag = false;
                    break;
                }
            }
        }
        return flag;
    }
};
```

## （2）递归

定义一个函数，通过递归实现是否为子结构；主函数，同样递归遍历树A。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        
        if (!A || !B)
        {
            return false;
        }
        if (isStruct(A, B))
        {
            return true;
        }

        return isSubStructure(A->left, B) || isSubStructure(A->right, B);
    }
    //判断是否是子结构，递归完成
    bool isStruct(TreeNode* A, TreeNode* B)
    {
        if (!B) return true;//B为空，B遍历完
        if (!A || A->val != B->val) return false;//A为空，B不为空，错误;两者值不相等，错误。
        
        return isStruct(A->left, B->left) && isStruct(A->right, B->right);
    }
    
};
```