# 题目：剑指 Offer 36. 二叉搜索树与双向链表

**难度**： 中等

**题目**：

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

为了让您更好地理解问题，以下面的二叉搜索树为例：

![image-20210627144827697](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210627144827697.png)



我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。



 ![image-20210627144852461](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210627144852461.png)

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。



注意：本题与主站 426 题相同：https://leetcode-cn.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/

注意：此题对比原题有改动。





来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

将二叉搜索树转换为排序的双向循环链表，因为二叉排序树以中序遍历的方式访问就是排序后的数字，所以对二叉树中序遍历。

设置一虚拟头结点head，tmp = head，在中序遍历中：

- `tmp->right = p`，前一个结点的后继指向下一个结点；
- `p->left = tmp`，后一个结点的前驱指向前一个结点；
- `tmp = tmp->right`，往后偏移

当然，第一个结点无前驱和最后一个结点无后继，所以之后还需要配置这两个。

ps：关键思想是，中序遍历某个元素时，比如1，此时tmp尚不是1，即访问完才是1。

# 解题代码


```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        
        if(!root) return root;

        Node* head = new Node(-1);
        Node* tmp = head;

        stack<Node*> stk;
        Node* p = root;
        while (!stk.empty() || p)
        {
            if (p)
            {
                stk.push(p);
                p = p->left;
            }
            else
            {
                p = stk.top();
                stk.pop();
                /*操作*/
                //从虚拟头结点开始操作，遍历到5时其实tmp只是4
                tmp->right = p;
                p->left = tmp;
                tmp = tmp->right;
                /*操作*/
                p = p->right;
            }
        }
        //头结点和尾结点的前驱和后继
        Node* ptemp = head->right;
        ptemp->left = tmp;
        tmp->right = ptemp;

        return head->right;
    }
    
};
```


