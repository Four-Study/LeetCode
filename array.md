### Two Sum

You may assume that each input would have ***exactly* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

This is my solution. 

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
```

This is the best solution I found. 

```python
class Solution:
   def twoSum(self, nums: List[int], target: int) -> List[int]:
       seen = {}
       for i, value in enumerate(nums): #1
           remaining = target - nums[i] #2
           
           if remaining in seen: #3
               return [i, seen[remaining]]  #4
           else:
               seen[value] = i  #5
```

I think my solution is quite naive by using double for loops. The good solution here makes use of dictionary in python. When we loop through the array, we can create a dictionary to store the keys and values in the reversed order!

### TwoSumII

This is just a variation of the problem twoSum.

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        seen = {}

        for i, value in enumerate(numbers): #1
        # calculate the number that is needed
            remaining = target - numbers[i] #2

            if remaining in seen: #3
                # This is the only line I modified. I switched the order of the 
                # two indexes, and add one to both of them
                return [seen[remaining] + 1, i + 1]  #4
            else:
                seen[value] = i  #5
```

### 3 Sum

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

### Remove Duplicates from Sorted Array

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**. Then return *the number of unique elements in* `nums`.

Consider the number of unique elements of `nums` to be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the unique elements in the order they were present in `nums` initially. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

```c++
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.

I made the following solution, which is not elegant at all. Because I have the double loop, which makes it out of time limit.

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        n = len(nums)
        k = 1
        for _ in range(n-1):
            if nums[k-1] == nums[k]:
                for i in range(k, n-1):
                    nums[i] = nums[i + 1]
            else:
                k = k + 1
        return k
```

Here is the solution i copied. 

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        j = 1
        for i in range(1, len(nums)):
            if nums[i] != nums[i - 1]: ## this captures each unique element essentially
                nums[j] = nums[i]
                j += 1
        return j
```

### Remove Duplicates from Sorted Array II

Given an integer array `nums` sorted in **non-decreasing order**, remove some duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears **at most twice**. The **relative order** of the elements should be kept the **same**.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` *after placing the final result in the first* `k` *slots of* `nums`.

Do **not** allocate extra space for another array. You must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

```sql
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        count = {}
        k = 0
        for num in nums:
            count[num] = count.get(num, 0) + 1
            if count[num] <= 2:
                nums[k] = num
                k += 1
            
        return k
```

In this kind of remove duplicates problem, do not worry about messing up with the array. It will work as long as it traverse each element. 

### Remove Element

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

After I studied from the last problem, I obtained the following answer quite soon.

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        j = 0
        for i in range(len(nums)):
            if nums[i] != val:
                nums[j] = nums[i]
                j = j + 1
        return j
```

### Plus One

You are given a **large integer** represented as an integer array `digits`, where each `digits[i]` is the `ith` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading `0`'s.

Increment the large integer by one and return *the resulting array of digits*.

My answer is okay. I tried to calculate the number first then convert it into a string, and an array.

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        n = len(digits)
        num_array = [digits[i] * 10**(n-i-1) for i in range(n)]
        num = sum(num_array)
        digits_new = [int(digit) for digit in str(num+1)]
        return digits_new
```

The online answer is different:

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:

        for i in range(len(digits) - 1, -1, -1):

            if digits[i] + 1 != 10:
                digits[i] += 1
                return digits
            
            digits[i] = 0

            if i == 0:
                return [1] + digits
```

### Merge Sorted Array

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be *stored inside the array* `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        midx = m - 1
        nidx = n - 1
        right = m + n - 1
        # if there is nothing left in nums2, it means the merge has been completed.
        while nidx >= 0: 
            # if there is nothing left in nums1, we should fill in the rest positions with nums2.
            if midx >= 0 and nums1[midx] > nums2[nidx]: 
                nums1[right] = nums1[midx]
                midx -= 1
            else:
                nums1[right] = nums2[nidx]
                nidx -= 1
            right -= 1
```

I did not come up with this answer, and it took me long to understand it. The checks of `midx` and `nidx` are not only for the two purposes I put there. They are also the guarantees for running the program. 

### Rotate Array

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

My stupid solution here:

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        k %= len(nums)
        temp = []
        for i in range(1,k+1):
            temp.append(nums[-i])
        print(temp)
        n = len(nums)
        for i in range(n-k):
            nums[n-1-i] = nums[n-k-1-i]

        for i in range(k):
            nums[i] = temp.pop()
```

**I did not add the line `k %= len(nums)`.** This is super important to ensure it's right when `k>len(nums)`. Check Leetcode, there are 3 ways to solve it. 

One of the solutions makes use of the math thinking reversing. I really like this kind of idea:

```python
class Solution:
    def reverse(self, nums: List[int], start: int, end: int) -> None:
        while start < end:
            nums[start], nums[end] = nums[end], nums[start]
            start += 1
            end -= 1

    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        k %= len(nums)
        n = len(nums)

        self.reverse(nums, 0, n-1)
        self.reverse(nums, 0, k-1)
        self.reverse(nums, k, n-1)
```

Another solution is also beautiful but it uses too much space:

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)
        a = [0] * n
        for i in range(n):
            a[(i + k) % n] = nums[i]

        nums[:] = a
```

### Jump Game II

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

- `0 <= j <= nums[i]` and
- `i + j < n`

Return *the minimum number of jumps to reach* `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        near = far = jumps = 0
        n = len(nums)
        while far < n - 1:
            farthest = 0
            for i in range(near, far + 1):
                farthest = max(farthest, i + nums[i])

            near = far + 1
            far = farthest
            jumps += 1

        return jumps
```

All solutions use greedy algorithm to check for the farthest position to jump. The reason I couldn't solve it is: I am not sure this is the optimal way to do so. 

### Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        min_price = float('inf')
        for i in range(len(prices)):
            if prices[i] < min_price:
                min_price = prices[i]
            if prices[i] - min_price > profit:
                profit = prices[i] - min_price
        return profit
```

This is called an easy question, but I didn't find it easy. 
