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

### Palindrome Number

Given an integer `x`, return `true` *if* `x` *is a* ***palindrome***, *and* `false` *otherwise*.

My naive solution: 

```python
class Solution(object):
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        # Convert the number to string
        x_str = str(x)
        # Check if the string is equal to its reverse
        return x_str == x_str[::-1]
```

Good one:

```python
class Solution(object):
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False

        inputNum = x
        newNum = 0
        while x > 0:
            newNum = newNum * 10 + x % 10 # This line extracts the last digit of x and add it to newNum. For example, if x is 123, then newNum is 3
            x = x//10 # This line removes the last digit from x by performing integer division by 10. For example, if x is 123, then after x//10, x becomes 12. 
        return newNum == inputNum
```

### Roman to Integer

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`. Given a roman numeral, convert it to an integer.

 I couldn't figure it out, so I looked up the solution directly.

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        translations = {
            "I": 1,
            "V": 5,
            "X": 10,
            "L": 50,
            "C": 100,
            "D": 500,
            "M": 1000
        }
        number = 0
        s = s.replace("IV", "IIII").replace("IX", "VIIII")
        s = s.replace("XL", "XXXX").replace("XC", "LXXXX")
        s = s.replace("CD", "CCCC").replace("CM", "DCCCC")
        for char in s:
            number += translations[char]
        return number
```

I think there are two points I should think more about:

1. The use of dictionary is really frequent in python.
2. Replace the subtraction with addition, then others are simple

### Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

For this question, I am proud of myself because I did not give up and get the answer which beats 100% users in terms of running time. Thus, I did not record any reference answer here. 

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        prefix = list(strs[0]) # take the first string as the prefix
        for string in strs:
            str_arr = list(string) # use built-in list function to create a list of characters
            if len(prefix) > len(str_arr): 
                # check the length of prefix, make sure it is shorter than any string in the array
                prefix = prefix[:len(str_arr)]
            for i in range(len(prefix)):
                if str_arr[i] != prefix[i]:
                    # if the characters do not match, we cut it.
                    prefix = prefix[:i]
                    break
        return ''.join(prefix) # this command to rejoin the array back to a string
```

I think I have learned two important operations here:

1. `list(str)` to make an array
2. `''.join()` to make a string

### Valid Parentheses

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

My own answer is fine, but it needs to be polished:

```python
class Solution:
    def isValid(self, s: str) -> bool:
        dictionary = {'(': ')', '[': ']', '{': '}'}
        stack = []
        for char in s:
            if char in dictionary:
                stack.append(char)
            else:
                if len(stack) == 0:
                    return False
                else:
                    last = stack.pop()
                    if dictionary[last] != char:
                        return False
                    
        return not stack 
```

1. When we want to loop over the elements in a string, we do not need to do list(string) every time. `for char in string` also works.

### Merge Two Sorted Lists

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

This question is super hard for me because I am not familiar with linked list. I did not work it out, so I just copied the answer online:

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode()
        pointer = dummy
        while list1 and list2:
            if list1.val <= list2.val:
                pointer.next = list1
                list1 = list1.next
            else:
                pointer.next = list2
                list2 = list2.next
            pointer = pointer.next
        if list1:
            pointer.next = list1
        else:
            pointer.next = list2
        return dummy.next
```

There are several things I can summarize:

1. `None` can be used in condition statement directly!
2. There is a difference between `&`  and `and`!

| Aspect               | `&`                                            | `and`               |
| -------------------- | ---------------------------------------------- | ------------------- |
| **Type**             | Bitwise operator                               | Logical operator    |
| **Operands**         | Works with integers, Booleans, or NumPy arrays | Works with any type |
| **Short-circuiting** | No                                             | Yes                 |
| **Result**           | A computed value (bitwise or element-wise)     | One of the operands |

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

### Find the Index of the First Occurrence in a String

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

 

**Example 1:**

```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
```

**Example 2:**

```
Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
```

This question is not that hard, but I could not solve it. So I just copied the answer over here.

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:

        if len(haystack) < len(needle):
            return -1

        for i in range(len(haystack)):
            if haystack[i:i+len(needle)] == needle:
                return i

        return -1 
```

I was thinking about using two loops to check one by one, that was stupid. 

This solution can work because you can actually slice string as if it is an array!

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

