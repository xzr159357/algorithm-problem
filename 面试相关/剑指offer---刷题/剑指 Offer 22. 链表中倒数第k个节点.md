# 题目：剑指 Offer 22. 链表中倒数第k个节点
**难度**： 简单

**题目**：

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。



## 示例 ：

```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>



# 解题代码

## （1）两次遍历

第一次遍历，计算链表的长度；第二次，通过链表长度减去k，求得往后偏移的次数，返回偏移后的head。


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
    ListNode* getKthFromEnd(ListNode* head, int k) {

        int len = 0;
        ListNode* tmp = head;
        while (tmp)
        {
            tmp = tmp->next;
            len++;
        }
        int t = len - k;
        for (int i = 0; i < t; i++)
        {
            head = head->next;
        }
        return head;
    }
};
```

## （2）双指针

利用两个指针标志位置，两个指针的位置之差为k，后一个指针对应位置的链表节点即为所求。

当然，其实也可以某个位置先往前走k步，然后两者就可以同步走了。

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
    ListNode* getKthFromEnd(ListNode* head, int k) {

        int left = 0, right = 0;
        ListNode* tmp = head;
        while (head)
        {
            if (right - left >= k)//两个指针的差距为k
            {
                left++;
                tmp = tmp->next;
            }
            right++;
            head = head->next;
        }
        return tmp;
    }
};
```

