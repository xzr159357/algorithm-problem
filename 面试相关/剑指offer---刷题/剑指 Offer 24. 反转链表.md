# 题目：剑指 Offer 24. 反转链表
**难度**： 简单

**题目**：

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。



## 示例 1：

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```





## 说明：

- `0 <= 节点个数 <= 5000`

  



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

- 可以利用队列存储偶数，然后遇到奇数时，如果队列有元素，说明有偶数在奇数左边，交换元素，并将交换的元素继续加入队列，这样只要遇到奇数在偶数右边就会交换，也就完成了。
- 双指针，一个指向头，一个指向尾，头每次找偶数，尾找奇数，两者交换，如此往复。
- 快慢指针，fast从头开始寻找奇数，找到奇数后就和slow指针位置元素交换。理论上，有多少个奇数就需要交换多少次，而slow从0开始，所以slow也等于奇数的个数，所以奇数也就都在左边了。

# 解题代码

## （1）头插法

要想实现链表的反转，可以每次往头部插入，这样最后一个元素就到了第一个，实现反转：

- 首先，定义一个虚拟头结点，每次插入都往虚拟头结点插入；
- 注意链表的偏移，即head=head->next的位置，在头部插入之后，同时要在头部插入头部后面的数据之后。


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
    ListNode* reverseList(ListNode* head) {

        ListNode* newHead = new ListNode(-1);
        
        while (head)
        {
            //每个节点都往头部插入，实现反转
            ListNode* tmp = newHead->next;
            newHead->next = head;//头插入
            head = head->next;//这一步，必须在此位置，往后偏移
            newHead->next->next = tmp;//插入结点的下一个结点的配置
        }
        return newHead->next;
    }
};
```

## （2）递归

如果递归实现，那么我们递归到倒数第二个结点，当前结点的下一个结点是倒数第一个结点，倒数第一个结点的下一个结点是倒数第二个结点就实现逆序，所以head->next->next = head，同时head->next = nullptr，即最后一个结点（逆序后）的下一个结点是nullptr。

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
    ListNode* reverseList(ListNode* head) {

        //0或1个元素，直接返回;同时也是递归返回条件
        if (!head || !head->next)
        {
            return head;
        }

        ListNode* newHead = reverseList(head->next);
        //当前结点的下一节点的下一个结点是当前结点
        head->next->next = head;
        head->next = nullptr;
        return newHead;
    }
};
```
