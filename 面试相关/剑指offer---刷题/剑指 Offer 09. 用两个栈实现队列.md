# 题目：剑指 Offer 09. 用两个栈实现队列
**难度**： 简单

**题目**：
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )



## 示例 1：

```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

## 示例 2：

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

## 提示：

- 1 <= values <= 10000

- 最多会对 appendTail、deleteHead 进行 10000 次调用

  

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
栈后进先出，队列先进后出，所以使用两个栈实现队列，我们可以定义一个栈stk1做输入，一个栈stk2做输出。输入时加入stk1中，这时候数据在栈顶，当我们输出时将输入栈stk1的数组逐个加入skt2输出栈中，头部的数据就到底下了，而之前头部数据就在栈顶了，操作就和队列一致了。

# 解题代码


```cpp
class CQueue {
private:
    stack<int> stk1;
    stack<int> stk2;
public:
    CQueue() {

    }
    
    void appendTail(int value) {

        stk1.push(value);
    }
    
    int deleteHead() {

        if (stk1.empty() && stk2.empty())
            return -1;
        if (stk2.empty())
        {
            while(!stk1.empty())
            {
                stk2.push(stk1.top());
                stk1.pop();
            }
        }
        int tmp = stk2.top();
        stk2.pop();
        return tmp;
    }
};
```



