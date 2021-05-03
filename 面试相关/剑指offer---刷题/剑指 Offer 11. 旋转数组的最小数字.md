# 题目：剑指 Offer 11. 旋转数组的最小数字
**难度**： 简单

**题目**：
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。







## 示例 1：

```
输入：[3,4,5,1,2]
输出：1
```

## 示例 2：

```
输入：[2,2,2,0,1]
输出：0
```



## 提示：

- `0 <= n <= 100`

  

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
<br>
<br>

# 解题思路
- 因为旋转数组之前是升序排列的，那么如果我们采用二分法，两部分中必然有一部分是有序的或者都是有序的。实际上，最小值必然出现在无序的位置。
- 所以，有以下几步：
  - 首先，判断整个数组是否已经有序，即是否有过旋转。
  - 接着，按二分法，找到无序部分，截取无序部分。
  - 不断重复第二步。
- 其中，判断mid和两边有两种，可以是从left或者从right，从right可以不要第一步即判断整个数组是否有序。
- 如上提到的**有序**，即递增还是递减，判断的符号也要相反。与原数组是递增还是递减有关，也和求最大值还是最小值有关。对应，递增数组求最小值，递减数组求最大值（在前者基础上符号相反即可）。
- 关键：改法用于求解递增找最小、递减找最大，如果是递增找最大，算法更加复杂。

# 解题代码


```cpp
class Solution {
public:
    int minArray(vector<int>& numbers) {

        /*
        sort(numbers.begin(), numbers.end());
        return numbers[0];
        */
        /*
        int res = INT_MAX;
        for (auto num : numbers)
            res = min(res, num);
        return res;
        */

        /*
        int n = numbers.size();
        int left = 0, right = n - 1;

        while (left < right)
        {
            //判断是否已经有序了
            if (numbers[left] < numbers[right]) return numbers[left];

            int mid = (left + right) / 2;
            if (numbers[left] < numbers[mid])//左边有序,不存在最大值，往右取
            {
                left = mid + 1;
            }
            else if (numbers[left] == numbers[mid])//相等，因为我们从左往右判断，left++
            {
               left++;
            }
            else
            {
                right = mid;
            }
        }
        return numbers[left];
        */
        //从右往左判断
        int n = numbers.size();
        int left = 0, right = n - 1;
        while (left < right)
        {
            int mid = (left + right) / 2;
            if (numbers[mid] < numbers[right])
            {
                right = mid;
            }
            else if (numbers[mid] > numbers[right])
            {
                left = mid + 1;
            }
            else
            {
                right--;
            }
        }
        return numbers[left];

    }
};
```



