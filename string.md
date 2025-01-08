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
