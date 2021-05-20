# 题目：剑指 Offer 25. 合并两个排序的链表
**难度**： 简单

**题目**：

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。



## 示例 1：

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```





## 说明：

- `0 <= 链表长度 <= 1000`

  



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

很容易想到，利用双指针法，哪个小就插入哪个数据，对应指针偏移即可，之后在处理未完成的数据。

# 解题代码

## （1）双指针


```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {

        ListNode *newHead = new ListNode(-1);
        ListNode* tmp = newHead;
        while (l1 && l2)
        {
            if (l1->val < l2->val)
            {
                tmp->next = l1;
                tmp = tmp->next;
                l1 = l1->next;
            }
            else
            {
                tmp->next = l2;
                tmp = tmp->next;
                l2 = l2->next;
            }
        }
        //未完数据
        tmp->next = l1 ? l1 : l2;

        return newHead->next;
    }
};
```

## （2）递归

递归，不用新的结点，直接在原节点的基础上修改。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {

        if (!l1) return l2;
        if (!l2) return l1;

        if (l1->val <= l2->val)
        {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }
        else
        {
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```