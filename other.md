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

### Kth Largest Element in an Array (Heap)

Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        heap = nums[:k]
        heapq.heapify(heap)

        for num in nums[k:]:
            if num > heap[0]:
                heapq.heappop(heap)
                heapq.heappush(heap, num)
        
        return heap[0]
```

I did not know how to solve this problem. The point here is to learn how to use heap structure in python. So we should remember some basic operations of `heapq` in python.

`heapq` is a Python module for heap queue (priority queue) algorithms, offering an efficient min-heap implementation.

- **`heappush(heap, item)`**: Adds `item` to `heap`, maintaining the heap property. 
  import heapq 
  heap = [] 
  heapq.heappush(heap, 3) 
  heapq.heappush(heap, 1) 
  print(heap)  # [1, 3]  

- **`heappop(heap)`**: Removes and returns the smallest item from `heap`. 
  smallest = heapq.heappop(heap) 
  print(smallest)  # 1  

- **`heappushpop(heap, item)`**: Pushes `item` to `heap`, then pops and returns the smallest item. 
  result = heapq.heappushpop(heap, 2) 
  print(result)  # 2  

- **`heapreplace(heap, item)`**: Pops and returns the smallest item, then pushes `item`. 
  replaced = heapq.heapreplace(heap, 5) 
  print(replaced)  # 3  

- **`heapify(x)`**: Transforms `x` into a heap in-place. 
  nums = [3, 1, 4] 
  heapq.heapify(nums) 
  print(nums)  # [1, 3, 4]  

- **`nlargest(n, iterable, key=None)`**: Returns the `n` largest elements. 
  largest = heapq.nlargest(2, [3, 1, 4]) 
  print(largest)  # [4, 3]  

- **`nsmallest(n, iterable, key=None)`**: Returns the `n` smallest elements. 
  smallest = heapq.nsmallest(2, [3, 1, 4]) 
  print(smallest)  # [1, 3]  

For a max-heap, use negative values: 

- **`heapq.heappush(heap, -item)`**

### Is Subsequence

Given two strings `s` and `t`, return `true` *if* `s` *is a **subsequence** of* `t`*, or* `false` *otherwise*.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

My solution looks dumb:

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if len(s) == 0:
            return True
        k = 0

        for i in range(len(t)):
            if s[k] == t[i]:
                k += 1

                if k == len(s):
                    break
        
        return k == len(s)
```

I like the two pointers solution:

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        sp = tp = 0

        while sp < len(s) and tp < len(t):
            if s[sp] == t[tp]:
                sp += 1
            tp += 1
        
        return sp == len(s)
```

