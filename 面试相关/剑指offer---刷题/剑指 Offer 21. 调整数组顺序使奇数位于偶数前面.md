# 题目：剑指 Offer 20. 表示数值的字符串
**难度**： 简单

**题目**：

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。



## 示例 1：

```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```





## 说明：

- `0 <= nums.length <= 50000`
- `1 <= nums[i] <= 10000`



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路

- 可以利用队列存储偶数，然后遇到奇数时，如果队列有元素，说明有偶数在奇数左边，交换元素，并将交换的元素继续加入队列，这样只要遇到奇数在偶数右边就会交换，也就完成了。
- 双指针，一个指向头，一个指向尾，头每次找偶数，尾找奇数，两者交换，如此往复。
- 快慢指针，fast从头开始寻找奇数，找到奇数后就和slow指针位置元素交换。理论上，有多少个奇数就需要交换多少次，而slow从0开始，所以slow也等于奇数的个数，所以奇数也就都在左边了。

# 解题代码

## （1）队列


```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {

        
        int n = nums.size();
        int i = 0;
        queue<int> que;
        
        while (i < n)
        {
            if (nums[i] & 1)//奇数
            {
                if(!que.empty())
                {
                    int k = que.front();
                    que.pop();
                    que.push(i);
                    int tmp = nums[k];
                    nums[k] = nums[i];
                    nums[i] = tmp;
                }
            }
            else
            {
                que.push(i);
            }
            i++;
        }
        return nums;
    }
};
```

## （2）双指针

```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {


        int n = nums.size();
        int left = 0, right = n - 1;
        
        while(left < right)
        {
            while (left < right && (nums[left] & 1)) left++;
            while (right > left && !(nums[right] & 1)) right--;
            
            swap(nums[left], nums[right]);
        }
        return nums;
        
        
    }
};
```

## （3）快慢指针

```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {


        int n = nums.size();
        int slow = 0, fast = 0;
        //快慢指针，fast一直往后找奇数，slow用于存储奇数
        
        while (fast < n)
        {
            if (nums[fast] & 1)//奇数，和slow位置交换
            {
                swap(nums[slow], nums[fast]);
                slow++;
            }
            fast++;
        }
        return nums;
        
        
    }
};
```

