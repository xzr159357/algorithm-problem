# 题目：剑指 Offer 37. 序列化二叉树

**难度**： 困难

**题目**：

请实现两个函数，分别用来序列化和反序列化二叉树。

你需要设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

提示：输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。



 

## 示例：

![image-20210629191640011](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210629191640011.png)

```
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]
```



注意：本题与主站 297 题相同：https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

二叉树的序列化和反序列化，就是能先转换为一个字符串，又能将字符串还原为二叉树。

- 首先，将二叉树转换为字符串时，将每个结点间用`,`分割，如果为空则用`null`表示。
- 将字符串转化为二叉树时，先将字符串根据`,`分割的字符串加入list中。
- 遍历list，先序遍历构建二叉树即可。

# 解题代码

## （1）DFS（先序遍历）


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
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string s;
        DFS(root, s);
        //cout << s << endl;
        return s;
    }
    void DFS(TreeNode* root, string& s)
    {
        if (!root)
        {
            s += "null,";
            return;
        }
        
        s += to_string(root->val) + ",";
        DFS(root->left, s);
        DFS(root->right, s);

    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        
        list<string> dataArray;
        string str;
        //以下将以','分割的字符存入list
        for (auto& ch : data)
        {
            if (ch == ',')
            {
                dataArray.push_back(str);
                str.clear();
            }
            else
            {
                str.push_back(ch);
            }
        }
        if (!str.empty())
        {
            dataArray.push_back(str);
            str.clear();
        }

        return rdeserialize(dataArray);
    }

    TreeNode* rdeserialize(list<string>& dataArray)
    {
        if (dataArray.front() == "null")
        {
            dataArray.erase(dataArray.begin());
            return nullptr;
        }

        TreeNode* root = new TreeNode(stoi(dataArray.front()));
        dataArray.erase(dataArray.begin());
        root->left = rdeserialize(dataArray);
        root->right = rdeserialize(dataArray);

        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```



## （2）BFS（层序遍历）

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
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string s;
        //层序遍历
        queue<TreeNode*> que;
        que.push(root);
        while (!que.empty())
        {
            int size = que.size();
            for (int i = 0; i < size; i++)
            {
                TreeNode* node = que.front();
                que.pop();
                if (node)
                {
                    s += to_string(node->val) + ",";
                    que.push(node->left);
                    que.push(node->right);
                }
                else
                {
                    s += "nullptr,";
                }
            }
        }
        //cout << s << endl;
        return s;
    }
    

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        
        list<string> dataArray;
        string str;
        for (auto ch : data)
        {
            if (ch == ',')
            {
                dataArray.push_back(str);
                str.clear();
            }
            else
            {
                str.push_back(ch);
            }
        }
        if (dataArray.front() == "nullptr")
        {
            return nullptr;
        }
        TreeNode* root = new TreeNode(stoi(dataArray.front()));
        TreeNode* node = root;
        dataArray.erase(dataArray.begin());
        queue<TreeNode*> que;
        que.push(node);
        
        while (!que.empty())
        {
            TreeNode* tmp = que.front();
            que.pop();
            if (dataArray.front() != "nullptr")
            {
                tmp->left = new TreeNode(stoi(dataArray.front()));
                que.push(tmp->left);
            }
            dataArray.erase(dataArray.begin());
            if (dataArray.front() != "nullptr")
            {
                tmp->right = new TreeNode(stoi(dataArray.front()));
                que.push(tmp->right);
            }
            dataArray.erase(dataArray.begin());
            
        }
        return root;
    }

    
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```

