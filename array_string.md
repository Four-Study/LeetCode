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

### Length of Last Word

Given a string `s` consisting of words and spaces, return *the length of the **last** word in the string.*

A **word** is a maximal substring consisting of non-space characters only.

My own answer is clean enough so I searched online.

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        s_arr = s.split()
        return len(s_arr[-1])
```

Two points to remember:

1. `.split()` splits the string by any white space by default. 
2. `array[-1]` slices the last element.

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

### Longest Substring Without Repeating Characters

Given a string `s`, find the length of the **longest** **substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left = max_length = 0
        char_set = set()
        for right in range(len(s)):
            while s[right] in char_set:
                char_set.remove(s[left])
                left += 1
            
            char_set.add(s[right])
            max_length = max(max_length, right - left + 1)
        return max_length
```

Point: As long as there is a repeating character, this string is done. 

## String Operations

- `str()`: Converts an object to a string.
  str(123)  # '123'

- `len()`: Returns the length of the string.
  len("hello")  # 5

- `startswith()`: Checks if the string starts with a specified prefix.
  "hello".startswith("he")  # True

- `endswith()`: Checks if the string ends with a specified suffix.
  "hello".endswith("lo")  # True

- `isalnum()`: Returns `True` if all characters are alphanumeric.
  "hello123".isalnum()  # True

- `isalpha()`: Returns `True` if all characters are alphabetic.
  "hello".isalpha()  # True

- `isdigit()`: Returns `True` if all characters are digits.
  "123".isdigit()  # True

- `islower()`: Checks if all characters are lowercase.
  "hello".islower()  # True

- `isupper()`: Checks if all characters are uppercase.
  "HELLO".isupper()  # True

- `isspace()`: Checks if all characters are whitespace.
  "   ".isspace()  # True

- `lower()`: Converts all characters to lowercase.
  "HELLO".lower()  # 'hello'

- `upper()`: Converts all characters to uppercase.
  "hello".upper()  # 'HELLO'

- `strip()`: Removes leading and trailing whitespace.
  "  hello  ".strip()  # 'hello'

- `lstrip()`: Removes leading whitespace.
  "  hello".lstrip()  # 'hello'

- `rstrip()`: Removes trailing whitespace.
  "hello  ".rstrip()  # 'hello'

- `replace()`: Replaces occurrences of a substring with another substring.
  "hello world".replace("world", "Python")  # 'hello Python'

- `join()`: Joins elements of an iterable into a single string.
  ",".join(["a", "b", "c"])  # 'a,b,c'

- `split()`: Splits the string into a list of substrings.
  "hello world".split()  # ['hello', 'world']

- `splitlines()`: Splits the string at line breaks.
  "hello\nworld".splitlines()  # ['hello', 'world']

- `partition()`: Splits the string into three parts: before, separator, after.
  "hello world".partition(" ")  # ('hello', ' ', 'world')

- `zfill()`: Pads the string with zeros to make it a certain length.
  "42".zfill(5)  # '00042'

- `format()`: Formats strings using placeholders.
  "Hello, {}".format("World")  # 'Hello, World'

- `f-strings`: Embeds expressions inside string literals (Python 3.6+).
  name = "World"
  f"Hello, {name}"
