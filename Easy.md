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
