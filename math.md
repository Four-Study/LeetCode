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

### Sqrt(x)

Given a non-negative integer `x`, return *the square root of* `x` *rounded down to the nearest integer*. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

My answer succeeded, however, it is bad in terms of time.

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        for i in range(x+1):
            if i * i > x:
                return i - 1
        return x
```

So I copied the better answer online. 

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        # if x == 0:
        #     return 0
        left, right = 1, x
        while left <= right:
            mid = (left + right) // 2
            if mid * mid == x:
                return mid
            elif mid * mid < x:
                left = mid + 1
            else:
                right = mid - 1
        return right
```

Key point: Search in log(n) time order is always nice!
