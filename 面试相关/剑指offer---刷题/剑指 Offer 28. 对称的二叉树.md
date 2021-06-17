# 题目：剑指 Offer 28. 对称的二叉树
**难度**： 简单

**题目**：

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

```
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3
```



## 示例 1：

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

## 示例 2：

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```



## 说明：

- `0 <= 节点个数 <= 1000`

注意：本题与主站 101 题相同：https://leetcode-cn.com/problems/symmetric-tree/



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

对称二叉树，就是判断二叉树镜像后是否相等。

# 解题代码

## （1）递归

递归的思路，在于使用双指针，一个往左、一个往右。


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
    bool isSymmetric(TreeNode* root) {

        return check(root, root);
    }

    bool check(TreeNode* root1, TreeNode* root2)
    {
        if (!root1 && !root2) return true;
        if (!root1 || !root2) return false;
        
        return root1->val == root2->val && check(root1->left, root2->right) && check(root1->right, root2->left);
    }

    
};
```

## （2）队列，非递归

非递归的思路，就是首先将root两次入队，每次队列都取出两个元素，进行各种判断；然后按一定规律将左、右子树入队，即要对比是否为镜像的那两个元素一起入队。

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
    bool isSymmetric(TreeNode* root) {

        queue<TreeNode*> que;
        que.push(root);
        que.push(root);
        while (!que.empty())
        {
            TreeNode* tmp1 = que.front();
            que.pop();
            TreeNode* tmp2 = que.front();
            que.pop();

            if (!tmp1 && !tmp2) continue;
            if (!tmp1 || !tmp2) return false;
            if (tmp1->val != tmp2->val) return false;

            que.push(tmp1->left);
            que.push(tmp2->right);
            que.push(tmp1->right);
            que.push(tmp2->left);
        }
        return true;
    }
};
```