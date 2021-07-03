# 题目：剑指 Offer 41. 数据流中的中位数

**难度**： 困难

**题目**：

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。
- double findMedian() - 返回目前所有元素的中位数。



 

## 示例 1：

```
输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
```

## 示例 2：

```
输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
```

## 提示：

- `最多会对 addNum、findMedian 进行 50000 次调用。`



注意：本题与主站 295 题相同：https://leetcode-cn.com/problems/find-median-from-data-stream/



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路



# 解题代码

## （1）插入排序


```cpp
class MedianFinder {
private:
    vector<int> arr;
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }
    
    void addNum(int num) {
        
        if (arr.empty())
        {
            arr.push_back(num);
        }
        else
        {
            arr.insert(lower_bound(arr.begin(), arr.end(), num), num);
        }
        
    }
    
    double findMedian() {

        int n = arr.size();
        return (n & 1) ? arr[n / 2] : (arr[n / 2 - 1] + arr[n / 2]) * 0.5;
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
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

