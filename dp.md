### Climbing Stairs (1D-DP)

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

I first used recursive method to calculate it. I think this is right, but the time limit is exceeded. I actually prefer this answer. 

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        elif n == 2:
            return 2
        else:
            return self.climbStairs(n-1) + self.climbStairs(n-2)
```

I utilized the `math` library in another solution. I like it less but it passed.

```python
import math
class Solution:
    def climbStairs(self, n: int) -> int:
        a = n
        b = 0
        s = 0
        while a >= 0:
            s += math.comb(a + b, a)
            a -= 2
            b += 1
        return s
```

### Longest Increasing Subsequence

Given an integer array `nums`, return *the length of the longest **strictly increasing*** ***subsequence***.

**Example 1:**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1] * len(nums)
        for i in range(1,len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j]+1)


        return max(dp)
```

**Realizing a Dynamic Programming Problem**

This problem has two important attributes that let us know it should be solved by dynamic programming. 
First, the question is asking for the maximum or minimum of something. 
Second, we have to make decisions that may depend on previously made decisions, which is very typical of a problem involving subsequences.
