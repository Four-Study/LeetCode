#### 3 Sum

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

When I was doing this problem, I know I have to use two layers of loops. However, I cannot think more.

I got an example: [-2, -1, 0, 0, 1, 2], which is very helpful for the understanding of this problem

```python
class Solution:
    def threeSum(self, nums):
        # res stands for results, used to contain all the 
        # triplets that satisfy the conditions
        res = []
        # sort the nums array for easier operation
        nums.sort()
        for i in range(len(nums)-2):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            l, r = i+1, len(nums)-1
            while l < r:
                s = nums[i] + nums[l] + nums[r]
                if s < 0:
                    l +=1 
                elif s > 0:
                    r -= 1
                else:
                    res.append((nums[i], nums[l], nums[r]))
                    while l < r and nums[l] == nums[l+1]:
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    l += 1; r -= 1
        return res
```

#### 4Sum

Given an array `nums` of `n` integers, return *an array of all the **unique** quadruplets* `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

I like this solution because it makes use of three sums.

```python
class Solution:
    def threeSum(self, nums, target):
        res = []
        for i in range(len(nums)-2):
            if i > 0 and nums[i] == nums[i-1]: continue
            j, k = i+1, len(nums)-1
            while j < k:
                total = nums[i] + nums[j] + nums[k]
                if total == target:
                    res.append([nums[i], nums[j], nums[k]])
                    while j < k and nums[j] == nums[j+1]: j += 1
                    while j < k and nums[k] == nums[k-1]: k -= 1
                    j += 1
                    k -= 1
                elif total < target:
                    j += 1
                else:
                    k -= 1
        return res

    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        res = []
        for i in range(len(nums)-3):
            if i > 0 and nums[i] == nums[i-1]: continue
            rest = self.threeSum(nums[i+1:], target-nums[i])
            res += [[nums[i]]+r for r in rest]
        return res
```
