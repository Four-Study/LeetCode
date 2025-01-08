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

### Is Subsequence (Two pointer)

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

### Majority Element (Hash table/Dictionary)

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        hash = {}
        count = m = 0
        for num in nums:
            
            hash[num] = hash.get(num, 0) + 1
            if hash[num] > count:
                m = num
                count = hash[num]

        return m
```

To be honest, this question is not hard but I cannot solve it. I could solve it if I know the function `hash.get`, which returns the count if it already exists in the dictionary and 0 if it does not exist. 
