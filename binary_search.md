### Search Insert Position (Binary Search)

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

### Find Peak Element

A peak element is an element that is strictly greater than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

Ignoring the `O(log n)` time requirement, I came up with a dumb solution:

```python
class Solution:
    def findPeakElement(self, nums):
        for i in range(len(nums) - 1):
            if nums[i] > nums[i + 1]:
                return i
        return len(nums) - 1
```

But the standard solution is:

```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        l = 0
        r = len(nums) - 1
        while l < r:
            mid = (l + r) // 2
            if nums[mid] > nums[mid + 1]:
                r = mid
            else:
                l = mid + 1
        return l
```

**It is hard to think why binary search would work in this case!**
