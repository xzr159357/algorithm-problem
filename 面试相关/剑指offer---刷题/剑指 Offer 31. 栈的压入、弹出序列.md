# 题目：剑指 Offer 31. 栈的压入、弹出序列

**难度**： 中等

**题目**：

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。



 

## 示例 1：

```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

## 示例 2：

```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```



## 提示：

- `0 <= pushed.length == popped.length <= 1000`
- `0 <= pushed[i], popped[i] < 1000`
- `pushed` 是 `popped` 的排列。



注意：本题与主站 946 题相同：https://leetcode-cn.com/problems/validate-stack-sequences/



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路



# 解题代码

## （1）栈+队列

创建一个栈和一个队列，从poped中选取第一个元素，以此元素在pushed中的位置为分割分别加入栈和队列。

而后对其进行遍历即可。比较复杂，法2更好。


```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {

        
        int n = popped.size();
        if (n == 0)
            return true;

        stack<int> stk;
        queue<int> que;
        auto res = find(pushed.begin(), pushed.end(), popped[0]);
        for (auto it = pushed.begin(); it != res; it++)
        {
            stk.push(*it);
        }
        for (auto it = res + 1; it != pushed.end(); it++)
        {
            que.push(*it);
        }


        for (int i = 1; i < n; i++)
        {
            int num = popped[i];
            if (!stk.empty())
            {
                if (stk.top() == num)
                {
                    stk.pop();
                    continue;
                }

                if (!que.empty())
                {
                    if (que.front() == num)
                    {
                        que.pop();
                        continue;
                    }
                    else
                    {
                        //如果既不等于栈顶又不等于堆顶，还有可能从队列中加元素至堆里
                        while (!que.empty() && que.front() != num)
                        {
                            stk.push(que.front());
                            que.pop();
                        }
                        if (!que.empty()) que.pop();
                        else return false;
                    }
                }
                else
                {
                    return false;
                }
            }
            else
            {
                //如果栈为空，从队列中取元素到栈中
                while (!que.empty() && que.front() != num)
                {
                    stk.push(que.front());
                    que.pop();
                }
                if (!que.empty()) que.pop();
            }
        }

        return true;
    }

};
```



## （2）模拟法

模拟压栈的过程，不断将pushed元素压入栈中，而后将栈顶与popped判断是否相等，相等则出栈，j++。

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {

        
        int n = popped.size();
        if (n == 0)
            return true;

        int j = 0;
        stack<int> stk;
        //模拟栈的压入过程
        for (auto num : pushed)
        {
            //将元素压入
            stk.push(num);
            //不断将pushed元素压入，栈顶等于弹出元素时弹出，j++
            while (!stk.empty() && j < n && stk.top() == popped[j])
            {
                j++;
                stk.pop();
            }
        }
        return j == n;
    }

};
```

