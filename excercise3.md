# Refactored Function Execution from Class Method

## Problem Statement
Refactor the function `fn` to execute any method from the `Caller` class using the argument `fn_to_call`, reducing the `fn` function to only one line.

## Solution
We utilized Python's built-in `getattr` function to fetch and execute the method of the `Caller` class based on the `fn_to_call` argument.

## Code

```python
class Caller:
    add = lambda a, b : a + b
    concat = lambda a, b : f'{a},{b}'
    divide = lambda a, b : a / b
    multiply = lambda a, b : a * b

def fn(fn_to_call, *args):
    return getattr(Caller, fn_to_call)(*args)
```

## How to Use
1. Instantiate the `Caller` class.
2. Call the `fn` function, passing the desired method's name as a string (e.g., 'add', 'concat', etc.) along with the required arguments.

Example:

```python
result = fn('add', 5, 3)
print(result)  # Output: 8

result = fn('concat', 'Hello', 'World')
print(result)  # Output: Hello,World
```
