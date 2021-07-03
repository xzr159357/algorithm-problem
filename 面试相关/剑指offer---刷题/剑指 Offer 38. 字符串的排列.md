# 题目：剑指 Offer 38. 字符串的排列

**难度**： 中等

**题目**：

输入一个字符串，打印出该字符串中字符的所有排列。

 

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。



 

## 示例：

```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```

## 提示：

- `1 <= s 的长度 <= 8`





来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

全排列问题，计算所有可能的情况。

# 解题代码

## （1）DFS（递归）

利用DFS循环遍历所有元素，注意不能有重复元素，所以在前面先将字符串`s`进行排序，那么遇到相邻元素时直接将其删除。


```cpp
class Solution {
public:
    vector<string> permutation(string s) {
        vector<string> data;
        string ans;
        sort(s.begin(), s.end());
        DFS(data, s, ans, s.size());
        return data;

    }
    void DFS(vector<string>& data, string s, string ans, int count)
    {
        if (count == ans.size())
        {
            data.push_back(ans);
            return;
        }
        
        for (int i = 0; i < s.size(); i++)
        {  
            if (i > 0 && s[i - 1] == s[i])
                continue;
            string str = s;
            str.erase(i, 1);
            DFS(data, str, ans + s[i], count);
        }
    }
};
```



## （2）两个堆

一个最大堆和一个最小堆：

- 最大堆中的所有数字都小于或等于最大堆的top元素（我们称之为 x）
- 最小堆中的所有数字都大于或等于最小堆的顶部元素（我们称之为y）
- 那么 x 和 y几乎小于（或等于）元素的一半，大于（或等于）另一半。这就是中值元素的定义。
- 最大堆存储较小的一半，最小堆存储较大的一半。两者的堆顶元素就是中值元素

```cpp
class MedianFinder {
private:
    priority_queue<int> lo;
    priority_queue<int, vector<int>, greater<int>> hi;
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }
    
    void addNum(int num) {
        
        lo.push(num);
        hi.push(lo.top());//最小堆加入最大堆中的最大值
        lo.pop();

        if (lo.size() < hi.size())
        {
            lo.push(hi.top());
            hi.pop();
        }
        
    }
    
    double findMedian() {

        return (lo.size() == hi.size()) ? (lo.top() + hi.top()) * 0.5 : lo.top();
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```





## （3）multiset和双指针

因为multiset会对插入元素进行排序，这时候我们需要用两个指针来判断，插入元素的位置，从而修改指针的偏移。插入元素前若为偶数，则插入元素后两个指针应该在同一个位置。

```cpp
class MedianFinder {
private:
    multiset<int> data;
    multiset<int>::iterator lo, hi;
public:
    
    /** initialize your data structure here. */
    MedianFinder() : lo(data.end()), hi(data.end()) {

    }
    
    void addNum(int num) {
        
        int n = data.size();//先计算再插入
        data.insert(num);
        

        if (!n)//插入一个元素后，两者指向头部元素
        {
            lo = hi = data.begin();
        }
        else if (n & 1)//插入num之前是奇数，lo和hi指向同一个位置
        {
            if (num < *lo)
            {
                lo--;
            }
            else
            {
                hi++;
            }
        }
        else
        {
            if (num > *lo && num < *hi)//介于lo和hi之间
            {
                //两者往中间偏
                lo++;
                hi--;
            }
            else if (num <= *lo)//位于lo左边，注意，这里关键，因为相同元素会插在相同元素的后面
            {
                lo = --hi;
            }
            else
            {
                lo++;
            }
        }

        
    }
    
    double findMedian() {
        
        int n = data.size();
        return (n & 1) ? *lo : (*lo + *hi) * 0.5;
        

    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

