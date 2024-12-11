# Python Basic Syntax

## Variables
- Variables are created by assigning values with `=`
  ```python
  x = 5
  y = "Hello, World!"
  z = 3.14
  ```

## Data Types
- Common data types: `int`, `float`, `str`, `bool`, `list`, `tuple`, `dict`, `set`
  ```python
  a = 10            # int
  b = 5.5           # float
  c = "Python"      # str
  d = True          # bool
  e = [1, 2, 3]     # list
  f = (4, 5, 6)     # tuple
  g = {"name": "Alice", "age": 25}  # dict
  h = {1, 2, 3}     # set
  ```

## Comments
- Single-line comments start with `#`
  ```python
  # This is a single-line comment
  ```
- Multi-line comments can be written using triple quotes
  ```python
  """
  This is a multi-line comment
  spanning several lines.
  """
  ```

## Operators
- Arithmetic: `+`, `-`, `*`, `/`, `%`, `//`, `**`
  ```python
    x = 10
    y = 3
    
    # Addition
    print(x + y)   # 13
    
    # Subtraction
    print(x - y)   # 7
    
    # Multiplication
    print(x * y)   # 30
    
    # Division
    print(x / y)   # 3.3333333333333335
    
    # Modulus (remainder)
    print(x % y)   # 1
    
    # Floor Division
    print(x // y)  # 3
    
    # Exponentiation
    print(x ** y)  # 1000
  ```
  
- Comparison: `==`, `!=`, `>`, `<`, `>=`, `<=`
  ```python
  x = 5
  y = 10
  print(x == y)  # False
  print(x < y)   # True
  ```

- Logical: `and`, `or`, `not`
  ```python
  a = True
  b = False
  print(a and b)  # False
  print(a or b)   # True
  ```

## Control Flow
### `if`, `elif`, `else`
```python
x = 10
if x > 15:
    print("Greater than 15")
elif x > 5:
    print("Greater than 5 but not 15")
else:
    print("5 or less")
```

## Loops
### `for` Loop
- Used for iteration over sequences (lists, strings, etc.)
  ```python
  for i in range(5):
      print(i)  # Prints 0, 1, 2, 3, 4
  ```

### `while` Loop
- Repeats while a condition is `True`
  ```python
  count = 0
  while count < 5:
      print(count)
      count += 1
  ```

## Functions
- Functions are defined with `def` and return values with `return`
  ```python
  def greet(name):
      return f"Hello, {name}!"
  
  print(greet("Alice"))
  ```

## Lists
### Basic Operations
- Adding elements to a list
  ```python
  fruits = ["apple", "banana"]
  fruits.append("cherry")
  fruits.extend(["orange", "grape"])
  ```

- Reverse the list

  ```python
  s = [1,2,3]
  reversed_s = s[::-1]
  ```

  

### Accessing Elements
- Indexing starts from 0
  ```python
  print(fruits[0])  # apple
  print(fruits[-1]) # grape
  ```

### Stack

```python
stack = []
# push
stack.append(1)
# pop
stack.pop()
# Check if the stack is empty
if not stack:
    print("Stack is empty")
```



## Dictionary
- Key-value pairs, accessed with keys
  ```python
  person = {"name": "Alice", "age": 25}
  print(person["name"])  # Alice
  ```
- Initialize an empty dictionary
  ```python
  empty_dict = {}
  ```

## List Comprehension
- Used to create lists in a concise way
  ```python
  squares = [x ** 2 for x in range(10)]
  ```

## Importing Modules
- Use `import` to bring in external modules
  ```python
  import math
  print(math.sqrt(16))  # 4.0
  ```

## Exception Handling
- Handling errors with `try`, `except`
  ```python
  try:
      value = 10 / 0
  except ZeroDivisionError:
      print("Division by zero error!")
  ```

## Classes and Objects
- Object-oriented programming is supported via classes
  ```python
  class Person:
      def __init__(self, name, age):
          self.name = name
          self.age = age
  
      def greet(self):
          print(f"Hello, my name is {self.name}.")
  
  alice = Person("Alice", 25)
  alice.greet()
  ```

## File Handling
- Opening and reading files
  ```python
  with open("file.txt", "r") as file:
      content = file.read()
      print(content)
  ```

## `lambda` Functions
- Small, anonymous functions
  ```python
  double = lambda x: x * 2
  print(double(5))  # 10
  ```

## Common Built-in Functions
- `print()`, `len()`, `range()`, `type()`, `input()`, etc.
  ```python
  print(len("Python"))  # 6
  ```

---
This list gives an overview of the most basic and frequently used Python syntax elements. It should help you get started or serve as a quick reference for common constructs.

## Tree



```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

result = []

def inorder(root):
    if not root:
        return []

    inorder(root.left)
    result.append(root.val)
    inorder(root.right)
    return result

def preorder(root):
    if not root:
        return []

    result.append(root.val)
    preorder(root.left)
    preorder(root.right)
    return result

def postorder(root):
    if not root:
        return []

    postorder(root.left)
    postorder(root.right)
    result.append(root.val)
    return result
```

- In-order: root in middle
- Pre-order: root first
- Post-order: root last

