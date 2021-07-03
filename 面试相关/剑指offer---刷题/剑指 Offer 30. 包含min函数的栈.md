# 题目：剑指 Offer 30. 包含min函数的栈

**难度**： 简单

**题目**：

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

 

## 示例 ：

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```



## 提示：

- 各函数的调用总次数不超过 20000 次

  

注意：本题与主站 155 题相同：https://leetcode-cn.com/problems/min-stack/



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

需要在栈中实现min函数的功能，需要使用双栈。一个栈保存数据，另一个栈和前一个同时存储当前位置的最小值。两者一起出栈，保证最小值是当前的。

# 解题代码



```cpp
class MinStack {
private:
    stack<int> stk;
    stack<int> stkMin;
public:
    /** initialize your data structure here. */
    MinStack() {
        stkMin.push(INT_MAX);
    }
    
    void push(int x) {
        stk.push(x);
        stkMin.push(::min(stkMin.top(), x));
    }
    
    void pop() {
        stk.pop();
        stkMin.pop();
    }
    
    int top() {
        return stk.top();
    }
    
    int min() {
        return stkMin.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```

