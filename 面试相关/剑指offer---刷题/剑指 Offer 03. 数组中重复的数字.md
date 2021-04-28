# 题目：633. 平方数之和
**难度**： 中等	

**题目**：
找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。



## 示例1
```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```


## 提示：
- 2 <= n <= 100000



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
（1）求重复数字，也就是第二个出现的数字，那么我们使用哈希表存储每个数字，每次新加入的数字和原哈希表匹配，存在相等找到，反之就像找。时间复杂度：O(n)，空间复杂度：O(n)。

（2）以上求解法，空间复杂度很大，而使用原地算法可以使空间复杂度降低至O(1)。具体思路，依据题意：所有数字都在 0～n-1 的范围内，如果数组不存在重复数字，那么值为i的数字必然在数组下表为i的位置（前提是已经排序好）。按照这个思路，我们每次遍历一个数字，**nums[i] != i** 时假设等于k，k应该出现在nums[k]的位置，所以我们将i和k位序的值交换。如果不重复的话n次后nums[i] = i；中间如果出现**nums[i] == nums[k]**，则说明出现重复，返回nums[i]。



# 解题代码
## （1）哈希表
```cpp
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {

        int n = nums.size();
        unordered_set<int> set;
        for (auto num : nums)
        {
            if(set.count(num))
                return num;
            else
                set.insert(num);
        }
        return -1;
        
    }
};
```



## （2）原地算法

```cpp
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {

        //依据题意，如果不存在重复数字，那么数字i必然在数组下标为i的位置,根据这一特点我们采取原地算法
        int n = nums.size();
        for(int i = 0; i < n; i++)
        {
            if(nums[i] != i)//不满足值与下表对应
            {
                if(nums[i] == nums[nums[i]])//下表i的值与下表i的值为下标对应的值
                {
                    return nums[i];
                }
                //将对应值交换到对应数组去，因为输入数组并没有排序
                int temp = nums[i];
                nums[i] = nums[temp];
                nums[temp] = temp;
            }
        }
        return -1;
    }
};
```



# 知识

unordered_set是一种**关联容器**，set和map内部实现是基于RB-Tree，是有序的，unordered_set和unordered_map是**基于hashtable**。是无序的。