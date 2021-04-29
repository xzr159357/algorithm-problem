# 题目：剑指 Offer 07. 重建二叉树
**难度**： 中等

**题目**：
输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```

    3

   / \
  9  20
    /  \
   15   7
```



## 限制：

- 0 <= 节点个数 <= 5000

  



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
- 前序遍历第一个结点是根节点，根节点的值对应到中序遍历相应的值，就能分割出中序遍历的左子树和右子树，自然左子树的长度和右子树的长度也知道了，而对应于前序遍历的长度也是一样的。所以，递归实现该操作，递归到下一层时同样相同的办法分割数组。

- 其中，可以先将中序遍历的值下标化，这样在得到前序遍历对应根节点值时就能轻松找到中序遍历根节点的下标了。

# 解题代码


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
    unordered_map<int, int> hashMap;
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {

        int n = preorder.size();
        int m = inorder.size();
        if(n == 0 || m == 0)
            return nullptr;
        
        for (int i = 0; i < m; i++)
        {
            hashMap[inorder[i]] = i;    //中序遍历的各值对应下表记录
        }

        return DFs(preorder, inorder, 0, n - 1,  0, n - 1);

        
    }
    //递归
    TreeNode* DFs(vector<int>& preorder, vector<int>& inorder, int preLeft, int preRight, int inLeft, int inRight)
    {
        if (preLeft > preRight)
            return nullptr;

        int preRoot = preLeft;//先序遍历的根节点
        int inRoot = hashMap[preorder[preRoot]];//中序遍历的根节点
        int leftSize = inRoot - inLeft;//左子树大小

        TreeNode* root = new TreeNode(preorder[preRoot]);//根节点
        root->left = DFs(preorder, inorder, preLeft + 1, preLeft + leftSize, inLeft, inRoot - 1);
        root->right = DFs(preorder, inorder, preLeft + leftSize + 1, preRight, inRoot + 1, inRight);
        return root;

    }
};
```



