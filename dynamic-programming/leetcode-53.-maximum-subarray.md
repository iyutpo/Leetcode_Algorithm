# Leetcode 53. Maximum Subarray

### Question:

Given an integer array `nums`, find the contiguous subarray \(containing at least one number\) which has the largest sum and return its sum.

**Example:**

```text
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

**Follow up:**

If you have figured out the O\(_n_\) solution, try coding another solution using the divide and conquer approach, which is more subtle.



这个问题有很多种解法，我们先来看Dynamic Programming，也就是最优解。

一般来说，对于这一类问题我们都可以用动态规划（Dynamic Programming）算法在O\(N\)时间内来求解。这类问题包括：

* 求  在给定大小的sliding window内的最大值（或最小值，或求和）

两种适用于标准DP算法的数组：

* 恒定的空间复杂度。沿着数组的方向对数组进行改动
* 线性的空间复杂度。先按从左到右的方向，然后从右到左的方向。最后将结果合并。

#### 例子：

#### Dynamic Programming

我们将用第一个方法为例，通过对当前的local maximum sum的不断更新，来寻找global maximum sum。假设我们有一个数组：`[-2, 1, -3, 4, -1, 2, 1, -5, 4]`。这个数组有正有负，我们想找到  和最大的 子数组。

**如果当前的 sum &lt; 0 我们就重新遍历。所以从左到右，当前sum分别为`[-2, 1, -2, 4, 3, 5, 6, 1, 5]`。**

然后我们再对 新得到的数组进行一次遍历，每个元素用当前看到的最大 local maximum sum进行替代，得到：**`[-2, 1, 1, 4, 4, 5, 6, 6, 6]`**。

详细代码如下：

```text
class Solution:
    def maxSubArray(self, nums):
        n = len(nums)
        max_sum = nums[0]
        for i in range(1, n):
            if nums[i - 1] > 0:
                nums[i] = nums[i] + nums[i-1]
            max_sum = max(nums[i], max_sum)
        return max_sum
# Example:
s = Solution
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
s.maxSubArray(nums)
>>> 6
```

```text
class Solution{
    public int maxSubArray(int[] nums){
        int n = nums.length;
        int curr_sum = nums[0], max_sum = nums[0];
        for (int i = 1; i < n; ++i){
            curr_sum = Math.max(nums[i], curr_sum + nums[i]);
            max_sum = Math.max(max_sum, curr_sum);
        }
        return max_sum;
    }
}
```



#### Greedy Algorithm

该问题也可以用贪婪算法求解（实际上和Dynamic Programming是一样的）。该算法可以在线性时间内求出 单一数组中的全局最大（最小）。该算法比较直观，我们可以遍历数组，然后在每一步更新如下信息：

* 当前的元素
* 当前的 local maximum sum
* 到目前为止的 global maximum sum

假设我们有一个数组：`[-2, 1, -3, 4, -1, 2, 1, -5, 4]`。这个数组有正有负，我们想找到  和最大的 子数组。

* `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`
* `[-2, 1, -2, 4,  3, 5, 6,  1, 5]`
* `[-2, 1,  1, 4,  4, 5, 6,  6, 6]`

详细代码如下：

```text
class Solution:
    def maxSubArray(self, nums):
        n = len(nums)
        max_sum = nums[0]
        for i in range(1, n):
            if nums[i - 1] > 0:
                nums[i] = nums[i] + nums[i-1]
            max_sum = max(nums[i], max_sum)
        return max_sum
# Example:
s = Solution
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
s.maxSubArray(nums)
>>> 6
```

```text
class Solution{
    public int maxSubArray(int[] nums){
        int n = nums.length;
        int curr_sum = nums[0], max_sum = nums[0];
        for (int i = 1; i < n; ++i){
            curr_sum = Math.max(nums[i], curr_sum + nums[i]);
            max_sum = Math.max(max_sum, curr_sum);
        }
        return max_sum;
    }
}
```



#### Divide and Conquer























