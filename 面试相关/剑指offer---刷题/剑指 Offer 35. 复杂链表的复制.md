# 题目：剑指 Offer 35. 复杂链表的复制

**难度**： 中等

**题目**：

请实现 `copyRandomList` 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 `next` 指针指向下一个节点，还有一个 `random` 指针指向链表中的任意节点或者 `null`。

 





## 示例 1：

![image-20210625100458790](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210625100458790.png)

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

## 示例 2：

![image-20210625100517019](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210625100517019.png)

```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```



## 示例 3：

![image-20210625100551434](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210625100551434.png)

```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```



## 实例4：

```
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。
```



## 提示：

- `-10000 <= Node.val <= 10000`
- `Node.random 为空（null）或指向链表中的节点。`
- 节点数目不超过 1000 。

注意：本题与主站 138 题相同：https://leetcode-cn.com/problems/copy-list-with-random-pointer/



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

该题需要实现深拷贝的功能，即指针域的内容也需要拷贝一份，而不仅仅是引用。

# 解题代码

## （1）哈希表


```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        
        Node* newHead = new Node(-1);
        Node* tmp = newHead;
        Node* pTmp =tmp->next;
        unordered_map<int, Node*> myMap;
        int ct;

        Node* p = head;
        unordered_map<Node*, int> value;
        ct = 0;
        //哈希表value记录：对应节点的指针的位置（int）
        while (p)
        {
            value[p] = ct;
            ct++;
            p = p->next;
        }
        ct = 0;
        while (head)
        {
            //遍历整个链表，当前元素在哈希表myMap中
            if (myMap.count(ct) == 0)
            {
                tmp->next = new Node(head->val);
                myMap[ct] = tmp->next;
            }   
            else
            {
                tmp->next = myMap[ct];
            }
            //如果有random，新建random加入哈希表
            if (head->random)
            {
                int num = value[head->random];
                cout << num << endl;
                if (myMap.count(num) == 0)
                {
                    tmp->next->random = new Node(head->random->val);
                    myMap[num] = tmp->next->random;
                }
                else
                {
                    tmp->next->random = myMap[num];
                }
            }
            tmp = tmp->next;
            head = head->next;
            ct++;
        }
        


        return newHead->next;
    }
};
```



## （2）改进版哈希表

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        
        Node* newHead = new Node(-1);
        Node* tmp = newHead;
        unordered_map<Node*, Node*> myMap;

        while (head)
        {
            if (myMap.count(head) == 0)
            {
                tmp->next = new Node(head->val);
                myMap[head] = tmp->next;
            }
            else
            {
                tmp->next = myMap[head];
            }
            if (head->random)
            {
                if (myMap.count(head->random) == 0)
                {
                    tmp->next->random = new Node(head->random->val);
                    myMap[head->random] = tmp->next->random;
                }
                else
                {
                    tmp->next->random = myMap[head->random];
                }
            }

            tmp = tmp->next;
            head = head->next;
        }
        return newHead->next;

    }
};
```



## （3）深度优先遍历

通过深度优先遍历，其实先遍历完所有元素，而后一直回退，直到回退到第一层。

同时，哈希表，通过将head--tmp元素一一对应，找到head就可以得到tmp。

```cpp
class Solution {
public:
    unordered_map<Node*, Node*> visited;
    Node* copyRandomList(Node* head) {
        
        if (!head) return nullptr;

        //已经访问创造过了
        if (visited.count(head) == 1) return visited[head];

        //没有访问过，创建新节点
        Node* tmp = new Node(head->val);
        visited[head] = tmp;
        //为当前结点复制next和random
        tmp->next = copyRandomList(head->next);
        tmp->random = copyRandomList(head->random);

        return tmp;
    }
};
```



## （4）空间O(1)

可以构建如下链表：`A->A'->B->B'->C->C'`，将拷贝的元素加在原来元素的右边。这时候，A'的random就是A的random的下一位了。

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        
        if(!head) return head;

        Node* ptr = head;
        //1.创建同head链表一模一样的链表在head右边，即A->A'->B->B'->C->C'
        while (ptr)
        {
            Node* node = new Node(ptr->val);
            //将node插入ptr和ptr->next之间
            node->next = ptr->next;
            ptr->next = node;
            ptr = node->next;   //node已经插入ptr和ptr->next之间，node->next就是原来的ptr->next
        }

        //2.为A'、B'、C'等元素添加random元素
        ptr = head;
        while (ptr)
        {
            //A'的random必然是A的random往后一位
            ptr->next->random = ptr->random ? ptr->random->next : nullptr;
            ptr = ptr->next->next;  //移动两位
        }

        //3.分离出A'->B'和A->B
        Node* ptrOld = head;
        Node* ptrNew = head->next;
        Node* res = ptrNew;//head->next也可
        while (ptrOld)
        {
            ptr = ptrNew->next;
            ptrNew->next = ptrNew->next ? ptrNew->next->next : nullptr;//注意考虑最后一个元素
            ptrOld->next = ptr;

            ptrOld = ptrOld->next;
            ptrNew = ptrNew->next;
        }

        return res;//注意，不可用head->next，一位head的next已经改变了
    }
};
```

