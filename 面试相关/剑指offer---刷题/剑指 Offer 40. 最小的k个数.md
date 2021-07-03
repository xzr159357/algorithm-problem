# 题目：剑指 Offer 40. 最小的k个数

**难度**： 中等

**题目**：

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

## 示例 1：

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

## 示例 2：

```
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

## 提示：

- `0 <= k <= arr.length <= 10000`
- `0 <= arr[i] <= 10000`

  

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

该题需要实现深拷贝的功能，即指针域的内容也需要拷贝一份，而不仅仅是引用。

# 解题代码

## （1）sort排序


```cpp
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        
        sort(arr.begin(), arr.end());
        vector<int> ans(arr.begin(), arr.begin() + k);
        return ans;

    }
};
```



## （2）大根堆

```cpp
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        

        vector<int> ans;
        if (k == 0)
            return ans;

        priority_queue<int> Q;
        for (int i = 0; i < k; i++)
        {
            Q.push(arr[i]);
        }

        for (int i = k; i < arr.size(); i++)
        {
            if (Q.top() > arr[i])
            {
                Q.pop();
                Q.push(arr[i]);
            }
        }

        for (int i = 0; i < k; i++)
        {
            ans.push_back(Q.top());
            Q.pop();
        }
        return ans;
    }
};
```



## （3）快速选择

- 借鉴快速排序的思想，只选择划分的一边。
- 快速选择算法c++的algorithm里面有nth_element

```cpp
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        
        srand((unsigned)time(NULL));
        randomized_selected(arr, 0, arr.size() - 1, k);
        vector<int> vec;
        for (int i = 0; i < k; i++)
        {
            vec.push_back(arr[i]);
        }
        return vec;
        
    }

    int partition(vector<int>& nums, int l, int r)
    {
        int pivot = nums[r];
        int i = l - 1;
        //遍历完，所有小于等于pivot的数都在左边
        for (int j = l; j < r; ++j)
        {
            //取比pivot小的数
            if (nums[j] <= pivot)
            {
                //先+后swap
                ++i;
                swap(nums[i], nums[j]);
            }
        }
        swap(nums[i + 1], nums[r]);
        return i + 1;//因为之前i = l - 1
    }

    //基于随机的划分
    int randomized_partition(vector<int>& nums, int l ,int r)
    {
        int i = rand() % (r - l + 1) + l;
        swap(nums[i], nums[r]);
        return partition(nums, l, r);
    }

    void randomized_selected(vector<int>& nums, int l, int r, int k)
    {
        if (l >= r) return;

        int pos = randomized_partition(nums, l, r);
        int num = pos - l + 1;
        if (k == num)
        {
            return;
        }
        else if (k < num)//太多了
        {
            randomized_selected(nums, l, pos - 1, k);
        }
        else
        {
            randomized_selected(nums, pos + 1, r, k - num);
        }
    }
};
```

