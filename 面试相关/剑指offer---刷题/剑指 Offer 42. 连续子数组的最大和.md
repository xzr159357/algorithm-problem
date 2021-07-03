# 题目：剑指 Offer 42. 连续子数组的最大和

**难度**： 简单

**题目**：

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

 

## 示例 1：

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

## 

## 提示：

- `1 <= arr.length <= 10^5`
- `-100 <= arr[i] <= 100`



注意：本题与主站 53 题相同：https://leetcode-cn.com/problems/maximum-subarray/



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路



# 解题代码

## （1）贪心算法


```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        int ans = INT_MIN;

        int sum = 0;
        for (auto num : nums)
        {
            sum += num;
            ans = max(ans, sum);
            if (sum < 0)
            {
                sum = 0;
            }
            
        }
        return ans;
    }
};
```



## （2）动态规划-滚动数组

动态规划，dp[i] = max(dp[i - 1] + num, num);，前一次加上当前元素或者当前元素中的大者，即是否要弃掉前面相加的元素。可以用滚动数组优化空间，只需一个变量即可。并且每次计算最大值ans，因为sum可能会丢掉最大值所在，每次判断时都会加上num值，所以需要另一个变量记录最大值。

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        int ans = nums[0];
        int sum = 0;
        for (auto num : nums)
        {
            sum = max(sum + num, num);//选择sum+num，或者弃掉前面元素，选择num
            ans = max(ans, sum);
        }
        return ans;
    }
};
```


