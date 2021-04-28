# 题目：剑指 Offer 06. 从尾到头打印链表
**难度**： 简单

**题目**：
输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

## 示例1

```
输入：head = [1,3,2]
输出：[2,3,1]
```



## 限制：

- 0 <= 链表长度 <= 10000

  



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
（1）首先将元素正序加入vector数组，之后新建一个数组从尾到头加入，利用rbegin()和rend()。

（2）利用栈**后进先出**的性质，首先将链表元素加入栈，这时候链表尾部元素从栈的头部开始加入数组。

（3）递归法，首先递归到链表的尾部，然后再将元素加入到数组，这时就是尾部到头部插入了。

（4）可以先计算链表的长度，之后往数组的尾部加入元素。

# 解题代码
## （1）vector法
```cpp
class Solution {
public:
    string replaceSpace(string s) {

        string str;
        int n = s.size();

        for(int i = 0; i < n; i++)
        {
            if(s[i] == ' ')
                str += "%20";
            else
                str += s[i];
        }
        return str;
        

    }
};
```



## （2）栈

```cpp
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {

        stack<int> stk;
        while(head)
        {
            stk.push(head->val);
            head = head->next;
        }

        vector<int> arr;
        while(!stk.empty())
        {
            arr.push_back(stk.top());
            stk.pop();
        }
        return arr;
    }
};
```



## （3）递归

```cpp
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {

        if (!head) return {};

        vector<int> arr = reversePrint(head->next);//一直递归到最后一层
        arr.push_back(head->val);//最后一层开始加入元素，从尾到头
        
        return arr;
    }
};
```



## 计算长度

```cpp
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {

        int len = 0;
        ListNode* temp = head;
        while (temp)
        {
            len++;
            temp = temp->next;
        }
        vector<int> arr(len);
        for(int i = len - 1; i >= 0; i--)
        {
            arr[i] = head->val;
            head = head->next;
        }
        return arr;
    }
};
```

