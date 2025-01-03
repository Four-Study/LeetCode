### Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

I solved this problem with some help from ChatGPT:

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        n = len(nums)
        if nums[n-1] < target:
            return n
        elif nums[0] > target:
            return 0
        elif nums[n // 2] == target:
            return n // 2
        elif nums[n // 2] > target:
            return self.searchInsert(nums[:(n//2)], target)
        else:
            return n // 2 + self.searchInsert(nums[(n//2):], target)
```

Keypoint: **`self` keyword**: It ensures that the `searchInsert` method is called as part of the current instance of the `Solution` class.

### Climbing Stairs

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

