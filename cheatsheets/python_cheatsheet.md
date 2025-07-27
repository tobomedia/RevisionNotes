# Python Cheatsheet

*A comprehensive reference for Python programming*

📚 **Official Documentation:** [Python Documentation](https://docs.python.org/3/)  
🐍 **Python Enhancement Proposals:** [PEP Index](https://peps.python.org/)

## Table of Contents

### 🚀 Getting Started
- [Installation & Setup](#installation--setup)
- [Basic Syntax](#basic-syntax)
- [Data Types](#data-types)
- [Variables & Constants](#variables--constants)

### 🎯 Core Language Features
- [Operators](#operators)
- [Control Flow](#control-flow)
- [Functions](#functions)
- [Classes & Objects](#classes--objects)

### 📊 Data Structures
- [Lists](#lists)
- [Dictionaries](#dictionaries)
- [Sets](#sets)
- [Tuples](#tuples)

### 🔧 Advanced Features
- [Comprehensions](#comprehensions)
- [Generators & Iterators](#generators--iterators)
- [Decorators](#decorators)
- [Context Managers](#context-managers)

### 📦 Modules & Packages
- [Imports](#imports)
- [Creating Modules](#creating-modules)
- [Package Management](#package-management)

### 🔍 Error Handling & Testing
- [Exception Handling](#exception-handling)
- [Testing](#testing)
- [Debugging](#debugging)

### 📁 File & I/O Operations
- [File Operations](#file-operations)
- [Working with JSON](#working-with-json)
- [Regular Expressions](#regular-expressions)

### 🌐 Popular Libraries
- [Standard Library](#standard-library)
- [Popular Third-Party Libraries](#popular-third-party-libraries)

### 📋 Best Practices
- [Code Style & PEP 8](#code-style--pep-8)
- [Performance Tips](#performance-tips)
- [Common Patterns](#common-patterns)

### 📚 Resources
- [Quick Reference Links](#quick-reference-links)

---

## Installation & Setup

📖 [Python Setup Documentation](https://docs.python.org/3/using/index.html)

```bash
# Check Python version
python --version
python3 --version

# Install packages
pip install package_name
pip install -r requirements.txt

# Virtual environments
python -m venv myenv
source myenv/bin/activate  # Linux/Mac
myenv\Scripts\activate     # Windows

# Package management with pip
pip list
pip freeze > requirements.txt
pip install --upgrade package_name
```

## Basic Syntax

📖 [Python Syntax Documentation](https://docs.python.org/3/reference/lexical_analysis.html)

```python
# Comments
# This is a single line comment

"""
This is a multi-line comment
or docstring
"""

# Basic output
print("Hello, World!")
print(f"Formatted string with {variable}")

# Input
name = input("Enter your name: ")
age = int(input("Enter your age: "))

# Indentation (4 spaces is standard)
if True:
    print("Indented block")
    if True:
        print("Nested indentation")

# Line continuation
long_string = "This is a very long string that " \
              "spans multiple lines"

# Multiple statements on one line (not recommended)
x = 1; y = 2; z = 3
```

## Data Types

📖 [Built-in Types Documentation](https://docs.python.org/3/library/stdtypes.html)

```python
# Numbers
integer = 42
float_num = 3.14159
complex_num = 1 + 2j

# Strings
single_quote = 'Hello'
double_quote = "World"
triple_quote = """Multi-line
string"""
raw_string = r"Raw string with \n no escape"
f_string = f"Formatted string with {variable}"

# Boolean
is_true = True
is_false = False

# None
empty_value = None

# Type checking
print(type(42))  # <class 'int'>
print(isinstance(42, int))  # True

# Type conversion
str(42)          # "42"
int("42")        # 42
float("3.14")    # 3.14
bool(1)          # True
list("hello")    # ['h', 'e', 'l', 'l', 'o']
```

## Variables & Constants

📖 [Naming Conventions - PEP 8](https://peps.python.org/pep-0008/#naming-conventions)

```python
# Variables (snake_case)
user_name = "john_doe"
total_count = 100
is_active = True

# Constants (UPPER_CASE)
MAX_SIZE = 1000
PI = 3.14159
DEFAULT_CONFIG = {
    "timeout": 30,
    "retries": 3
}

# Multiple assignment
x, y, z = 1, 2, 3
a = b = c = 0

# Swapping variables
x, y = y, x

# Unpacking
coordinates = (10, 20)
x, y = coordinates

# Global and local variables
global_var = "I'm global"

def my_function():
    local_var = "I'm local"
    global global_var
    global_var = "Modified global"
```

## Operators

📖 [Operators Documentation](https://docs.python.org/3/reference/expressions.html)

```python
# Arithmetic operators
result = 10 + 5    # Addition: 15
result = 10 - 5    # Subtraction: 5
result = 10 * 5    # Multiplication: 50
result = 10 / 5    # Division: 2.0
result = 10 // 3   # Floor division: 3
result = 10 % 3    # Modulo: 1
result = 2 ** 3    # Exponentiation: 8

# Comparison operators
10 == 5    # False
10 != 5    # True
10 > 5     # True
10 < 5     # False
10 >= 5    # True
10 <= 5    # False

# Logical operators
True and False  # False
True or False   # True
not True        # False

# Identity operators
x is y          # True if x and y are the same object
x is not y      # True if x and y are different objects

# Membership operators
'a' in 'apple'      # True
'z' not in 'apple'  # True

# Bitwise operators
5 & 3   # AND: 1
5 | 3   # OR: 7
5 ^ 3   # XOR: 6
~5      # NOT: -6
5 << 1  # Left shift: 10
5 >> 1  # Right shift: 2

# Assignment operators
x += 5   # x = x + 5
x -= 5   # x = x - 5
x *= 5   # x = x * 5
x /= 5   # x = x / 5
x **= 2  # x = x ** 2

# Walrus operator (Python 3.8+)
if (n := len(my_list)) > 10:
    print(f"List is long: {n} items")
```

## Control Flow

📖 [Control Flow Documentation](https://docs.python.org/3/tutorial/controlflow.html)

```python
# If statements
age = 18
if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teenager")
else:
    print("Child")

# Ternary operator
status = "adult" if age >= 18 else "minor"

# For loops
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

for i in range(1, 6):
    print(i)  # 1, 2, 3, 4, 5

for i in range(0, 10, 2):
    print(i)  # 0, 2, 4, 6, 8

# Iterate over sequences
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# While loops
count = 0
while count < 5:
    print(count)
    count += 1

# Loop control
for i in range(10):
    if i == 3:
        continue  # Skip to next iteration
    if i == 7:
        break     # Exit loop
    print(i)

# For-else and while-else
for i in range(5):
    print(i)
else:
    print("Loop completed normally")

# Match-case (Python 3.10+)
def handle_status(status):
    match status:
        case 200:
            return "OK"
        case 404:
            return "Not Found"
        case 500 | 502 | 503:
            return "Server Error"
        case _:
            return "Unknown Status"
```

## Functions

📖 [Functions Documentation](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)

```python
# Basic function
def greet(name):
    return f"Hello, {name}!"

# Function with default parameters
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

# Function with type hints (Python 3.5+)
def add(a: int, b: int) -> int:
    return a + b

# Variable arguments
def sum_all(*args):
    return sum(args)

def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

# Mixed parameters
def complex_function(required, *args, **kwargs):
    print(f"Required: {required}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

# Lambda functions
square = lambda x: x ** 2
add = lambda x, y: x + y
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))

# Higher-order functions
def apply_operation(func, x, y):
    return func(x, y)

result = apply_operation(lambda a, b: a * b, 5, 3)

# Nested functions and closures
def outer_function(x):
    def inner_function(y):
        return x + y
    return inner_function

add_5 = outer_function(5)
result = add_5(3)  # 8

# Recursive functions
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Generator functions
def countdown(n):
    while n > 0:
        yield n
        n -= 1

for num in countdown(5):
    print(num)
```

## Classes & Objects

📖 [Classes Documentation](https://docs.python.org/3/tutorial/classes.html)

```python
# Basic class
class Person:
    # Class variable
    species = "Homo sapiens"
    
    def __init__(self, name, age):
        # Instance variables
        self.name = name
        self.age = age
    
    def greet(self):
        return f"Hello, I'm {self.name}"
    
    def __str__(self):
        return f"Person(name='{self.name}', age={self.age})"
    
    def __repr__(self):
        return f"Person('{self.name}', {self.age})"

# Creating objects
person = Person("Alice", 30)
print(person.greet())
print(person)

# Inheritance
class Student(Person):
    def __init__(self, name, age, student_id):
        super().__init__(name, age)
        self.student_id = student_id
    
    def study(self, subject):
        return f"{self.name} is studying {subject}"
    
    # Method overriding
    def greet(self):
        return f"Hi, I'm {self.name}, student ID: {self.student_id}"

# Multiple inheritance
class Teacher:
    def teach(self, subject):
        return f"Teaching {subject}"

class TeachingAssistant(Student, Teacher):
    def __init__(self, name, age, student_id, course):
        super().__init__(name, age, student_id)
        self.course = course

# Property decorators
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def radius(self):
        return self._radius
    
    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius must be positive")
        self._radius = value
    
    @property
    def area(self):
        return 3.14159 * self._radius ** 2

# Class methods and static methods
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b
    
    @classmethod
    def from_string(cls, string_data):
        # Parse string and create instance
        return cls(*string_data.split(','))

# Abstract base classes
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def make_sound(self):
        pass

class Dog(Animal):
    def make_sound(self):
        return "Woof!"

# Data classes (Python 3.7+)
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int
    
    def distance_from_origin(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5

point = Point(3, 4)
print(point.distance_from_origin())  # 5.0
```

## Lists

📖 [Lists Documentation](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)

```python
# Creating lists
empty_list = []
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]
nested = [[1, 2], [3, 4], [5, 6]]

# List operations
numbers.append(6)           # Add to end
numbers.insert(0, 0)        # Insert at index
numbers.extend([7, 8, 9])   # Add multiple items
numbers.remove(5)           # Remove first occurrence
popped = numbers.pop()      # Remove and return last item
popped = numbers.pop(0)     # Remove and return item at index

# List methods
numbers.index(3)            # Find index of value
numbers.count(2)            # Count occurrences
numbers.sort()              # Sort in place
numbers.reverse()           # Reverse in place
copied = numbers.copy()     # Shallow copy

# List slicing
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(numbers[2:5])         # [2, 3, 4]
print(numbers[:3])          # [0, 1, 2]
print(numbers[3:])          # [3, 4, 5, 6, 7, 8, 9]
print(numbers[::2])         # [0, 2, 4, 6, 8]
print(numbers[::-1])        # Reverse [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

# List unpacking
first, *middle, last = [1, 2, 3, 4, 5]
# first = 1, middle = [2, 3, 4], last = 5

# List as stack
stack = []
stack.append(1)    # Push
stack.append(2)
item = stack.pop() # Pop

# List as queue (use collections.deque instead)
from collections import deque
queue = deque([1, 2, 3])
queue.append(4)         # Add to right
item = queue.popleft()  # Remove from left
```

## Dictionaries

📖 [Dictionaries Documentation](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)

```python
# Creating dictionaries
empty_dict = {}
person = {"name": "Alice", "age": 30, "city": "New York"}
person = dict(name="Alice", age=30, city="New York")

# Dictionary operations
person["email"] = "alice@example.com"  # Add/update
age = person["age"]                    # Access (KeyError if not found)
age = person.get("age", 0)             # Safe access with default
del person["city"]                     # Delete key
popped = person.pop("email", None)     # Remove and return

# Dictionary methods
keys = person.keys()        # dict_keys object
values = person.values()    # dict_values object
items = person.items()      # dict_items object

# Dictionary comprehension
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Filtering dictionary
filtered = {k: v for k, v in person.items() if isinstance(v, str)}

# Merging dictionaries (Python 3.9+)
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
merged = dict1 | dict2

# Update dictionary
person.update({"age": 31, "country": "USA"})

# Nested dictionaries
users = {
    "user1": {"name": "Alice", "permissions": ["read", "write"]},
    "user2": {"name": "Bob", "permissions": ["read"]}
}

# Default dictionaries
from collections import defaultdict
dd = defaultdict(list)
dd["fruits"].append("apple")  # Creates list automatically

# Counter
from collections import Counter
text = "hello world"
char_count = Counter(text)
print(char_count.most_common(3))  # [('l', 3), ('o', 2), ('h', 1)]
```

## Sets

📖 [Sets Documentation](https://docs.python.org/3/tutorial/datastructures.html#sets)

```python
# Creating sets
empty_set = set()  # Note: {} creates empty dict
numbers = {1, 2, 3, 4, 5}
from_list = set([1, 2, 3, 2, 1])  # {1, 2, 3}

# Set operations
numbers.add(6)          # Add element
numbers.update([7, 8])  # Add multiple elements
numbers.remove(5)       # Remove (KeyError if not found)
numbers.discard(5)      # Remove (no error if not found)
popped = numbers.pop()  # Remove and return arbitrary element

# Set operations (mathematical)
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

union = set1 | set2              # {1, 2, 3, 4, 5, 6}
intersection = set1 & set2       # {3, 4}
difference = set1 - set2         # {1, 2}
symmetric_diff = set1 ^ set2     # {1, 2, 5, 6}

# Set methods
is_subset = set1.issubset(set2)
is_superset = set1.issuperset(set2)
is_disjoint = set1.isdisjoint(set2)

# Set comprehension
squares = {x**2 for x in range(5)}  # {0, 1, 4, 9, 16}

# Frozen sets (immutable)
frozen = frozenset([1, 2, 3, 4])
```

## Tuples

📖 [Tuples Documentation](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences)

```python
# Creating tuples
empty_tuple = ()
single_item = (1,)  # Note the comma
coordinates = (10, 20)
person = ("Alice", 30, "Engineer")

# Tuple unpacking
x, y = coordinates
name, age, job = person

# Named tuples
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)  # 10 20

Person = namedtuple('Person', ['name', 'age', 'job'])
alice = Person("Alice", 30, "Engineer")
print(alice.name)  # Alice

# Tuple methods
numbers = (1, 2, 3, 2, 1)
count = numbers.count(2)    # 2
index = numbers.index(3)    # 2

# Tuples as dictionary keys
locations = {
    (0, 0): "origin",
    (1, 1): "northeast",
    (-1, -1): "southwest"
}

# Tuple slicing (same as lists)
numbers = (0, 1, 2, 3, 4, 5)
print(numbers[2:4])  # (2, 3)
```

## Comprehensions

📖 [List Comprehensions Documentation](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)

```python
# List comprehensions
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
words = ["hello", "world", "python"]
lengths = [len(word) for word in words]

# Nested list comprehensions
matrix = [[i + j for j in range(3)] for i in range(3)]
# [[0, 1, 2], [1, 2, 3], [2, 3, 4]]

flattened = [item for row in matrix for item in row]
# [0, 1, 2, 1, 2, 3, 2, 3, 4]

# Dictionary comprehensions
word_lengths = {word: len(word) for word in words}
squared_evens = {x: x**2 for x in range(10) if x % 2 == 0}

# Set comprehensions
unique_lengths = {len(word) for word in words}
vowels_in_words = {char for word in words for char in word if char in 'aeiou'}

# Generator expressions
squares_gen = (x**2 for x in range(10))
sum_squares = sum(x**2 for x in range(10))

# Conditional expressions in comprehensions
result = [x if x > 0 else 0 for x in [-1, 2, -3, 4]]
# [0, 2, 0, 4]

# Multiple conditions
filtered = [x for x in range(100) if x % 2 == 0 if x % 3 == 0]
# Multiples of 6
```

## Generators & Iterators

📖 [Generators Documentation](https://docs.python.org/3/tutorial/classes.html#generators)

```python
# Generator functions
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

# Using generators
fib_gen = fibonacci(10)
for num in fib_gen:
    print(num)

# Generator expressions
squares = (x**2 for x in range(10))
print(next(squares))  # 0
print(next(squares))  # 1

# Custom iterator class
class CountDown:
    def __init__(self, start):
        self.start = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.start <= 0:
            raise StopIteration
        self.start -= 1
        return self.start + 1

# Using the iterator
for num in CountDown(5):
    print(num)  # 5, 4, 3, 2, 1

# Built-in iterator functions
from itertools import count, cycle, repeat, chain, combinations, permutations

# Infinite iterators
counter = count(start=10, step=2)  # 10, 12, 14, 16, ...
cycler = cycle(['A', 'B', 'C'])    # A, B, C, A, B, C, ...
repeater = repeat('X', 3)          # X, X, X

# Finite iterators
list1 = [1, 2, 3]
list2 = [4, 5, 6]
chained = chain(list1, list2)      # 1, 2, 3, 4, 5, 6

# Combinatorics
items = ['A', 'B', 'C']
combos = list(combinations(items, 2))    # [('A', 'B'), ('A', 'C'), ('B', 'C')]
perms = list(permutations(items, 2))     # [('A', 'B'), ('A', 'C'), ('B', 'A'), ...]

# Generator with send() and close()
def echo_generator():
    while True:
        value = yield
        print(f"Received: {value}")

gen = echo_generator()
next(gen)  # Prime the generator
gen.send("Hello")  # Received: Hello
gen.close()        # Clean up
```

## Decorators

📖 [Decorators Documentation](https://docs.python.org/3/glossary.html#term-decorator)

```python
# Simple decorator
def my_decorator(func):
    def wrapper():
        print("Before function call")
        result = func()
        print("After function call")
        return result
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

# Decorator with arguments
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")

# Class-based decorators
class CountCalls:
    def __init__(self, func):
        self.func = func
        self.count = 0
    
    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call {self.count} of {self.func.__name__}")
        return self.func(*args, **kwargs)

@CountCalls
def my_function():
    print("Function executed")

# Built-in decorators
from functools import wraps, lru_cache
import time

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer
@lru_cache(maxsize=128)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Property decorators
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature cannot be below absolute zero")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32

# Static and class method decorators
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b
    
    @classmethod
    def from_string(cls, string):
        return cls(*map(int, string.split(',')))
```

## Context Managers

📖 [Context Managers Documentation](https://docs.python.org/3/reference/datamodel.html#context-managers)

```python
# Using built-in context managers
with open('file.txt', 'r') as f:
    content = f.read()
# File is automatically closed

# Custom context manager class
class DatabaseConnection:
    def __init__(self, host, port):
        self.host = host
        self.port = port
        self.connection = None
    
    def __enter__(self):
        print(f"Connecting to {self.host}:{self.port}")
        self.connection = f"Connection to {self.host}:{self.port}"
        return self.connection
    
    def __exit__(self, exc_type, exc_value, traceback):
        print("Closing connection")
        self.connection = None
        if exc_type is not None:
            print(f"Exception occurred: {exc_value}")
        return False  # Don't suppress exceptions

# Using custom context manager
with DatabaseConnection("localhost", 5432) as conn:
    print(f"Using {conn}")

# Context manager using contextlib
from contextlib import contextmanager

@contextmanager
def timer_context():
    start = time.time()
    try:
        yield start
    finally:
        end = time.time()
        print(f"Operation took {end - start:.4f} seconds")

# Using the context manager
with timer_context() as start_time:
    time.sleep(1)
    print(f"Started at {start_time}")

# Multiple context managers
with open('input.txt', 'r') as infile, open('output.txt', 'w') as outfile:
    content = infile.read()
    outfile.write(content.upper())

# Suppressing exceptions
from contextlib import suppress

with suppress(FileNotFoundError):
    with open('nonexistent.txt', 'r') as f:
        content = f.read()
# Continues execution even if file doesn't exist
```

## Imports

📖 [Import System Documentation](https://docs.python.org/3/reference/import.html)

```python
# Basic imports
import math
import os, sys  # Multiple imports (not recommended)

# Import with alias
import numpy as np
import pandas as pd

# Import specific items
from math import pi, sqrt, sin
from os import path, environ

# Import all (not recommended)
from math import *

# Relative imports (within packages)
from . import module_name          # Same package
from ..package import module_name  # Parent package
from .subpackage import module_name # Subpackage

# Conditional imports
try:
    import ujson as json
except ImportError:
    import json

# Import at runtime
import importlib
module_name = "math"
math_module = importlib.import_module(module_name)

# Lazy imports
def get_numpy():
    import numpy as np
    return np

# Check if module is available
try:
    import requests
    HAS_REQUESTS = True
except ImportError:
    HAS_REQUESTS = False

# Import with __future__
from __future__ import annotations  # For type hints
from __future__ import division     # Python 2/3 compatibility
```

## Creating Modules

📖 [Modules Documentation](https://docs.python.org/3/tutorial/modules.html)

```python
# mymodule.py
"""
A simple example module.

This module demonstrates basic module structure.
"""

__version__ = "1.0.0"
__author__ = "Your Name"

# Module-level constants
DEFAULT_TIMEOUT = 30
MAX_RETRIES = 3

# Module-level variables
_private_var = "Not intended for public use"
public_var = "Available to importers"

# Functions
def public_function(x, y):
    """Add two numbers together."""
    return x + y

def _private_function():
    """Private function - convention only."""
    return "private"

# Classes
class MyClass:
    def __init__(self, value):
        self.value = value
    
    def get_value(self):
        return self.value

# Module initialization code
if __name__ == "__main__":
    # Code that runs when module is executed directly
    print("Module executed directly")
else:
    print("Module imported")
```

```python
# Package structure
mypackage/
    __init__.py          # Makes it a package
    module1.py
    module2.py
    subpackage/
        __init__.py
        submodule.py

# __init__.py example
"""MyPackage - A sample Python package."""

from .module1 import important_function
from .module2 import ImportantClass

__version__ = "1.0.0"
__all__ = ["important_function", "ImportantClass"]
```

## Package Management

📖 [Installing Packages Documentation](https://docs.python.org/3/installing/index.html)

```bash
# pip basics
pip install package_name
pip install package_name==1.2.3    # Specific version
pip install package_name>=1.2.0    # Minimum version
pip install -r requirements.txt    # From requirements file
pip install -e .                   # Editable install (development)

# pip commands
pip list                           # List installed packages
pip show package_name              # Show package info
pip freeze > requirements.txt     # Export requirements
pip uninstall package_name        # Uninstall package
pip install --upgrade package_name # Upgrade package

# Virtual environments
python -m venv myenv              # Create virtual environment
source myenv/bin/activate         # Activate (Linux/Mac)
myenv\Scripts\activate            # Activate (Windows)
deactivate                        # Deactivate

# pipenv (alternative)
pip install pipenv
pipenv install requests           # Install package
pipenv install pytest --dev      # Development dependency
pipenv shell                      # Activate virtual environment
pipenv lock                       # Generate Pipfile.lock

# poetry (modern alternative)
pip install poetry
poetry new my-project            # Create new project
poetry add requests              # Add dependency
poetry add pytest --group dev   # Add dev dependency
poetry install                   # Install dependencies
poetry shell                     # Activate virtual environment
```

## Exception Handling

📖 [Errors and Exceptions Documentation](https://docs.python.org/3/tutorial/errors.html)

```python
# Basic try-except
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")

# Multiple exception types
try:
    value = int(input("Enter a number: "))
    result = 10 / value
except ValueError:
    print("Invalid input - not a number")
except ZeroDivisionError:
    print("Cannot divide by zero")

# Catching multiple exceptions
try:
    # risky code
    pass
except (ValueError, TypeError) as e:
    print(f"Input error: {e}")

# Generic exception handling
try:
    # risky code
    pass
except Exception as e:
    print(f"An error occurred: {e}")

# Try-except-else-finally
try:
    file = open("data.txt", "r")
    data = file.read()
except FileNotFoundError:
    print("File not found")
else:
    print("File read successfully")
    # Runs only if no exception occurred
finally:
    # Always runs
    if 'file' in locals():
        file.close()

# Raising exceptions
def validate_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 150:
        raise ValueError("Age seems unrealistic")
    return age

# Custom exceptions
class CustomError(Exception):
    """Custom exception class."""
    def __init__(self, message, error_code=None):
        super().__init__(message)
        self.error_code = error_code

class ValidationError(CustomError):
    """Raised when validation fails."""
    pass

# Using custom exceptions
try:
    raise ValidationError("Invalid data format", error_code=400)
except ValidationError as e:
    print(f"Validation failed: {e}")
    print(f"Error code: {e.error_code}")

# Exception chaining
try:
    try:
        1 / 0
    except ZeroDivisionError as e:
        raise ValueError("Invalid calculation") from e
except ValueError as e:
    print(f"Error: {e}")
    print(f"Caused by: {e.__cause__}")
```

## Testing

📖 [Testing Documentation](https://docs.python.org/3/library/unittest.html)

```python
# unittest module
import unittest

class TestMathOperations(unittest.TestCase):
    def setUp(self):
        """Set up test fixtures before each test method."""
        self.numbers = [1, 2, 3, 4, 5]
    
    def tearDown(self):
        """Clean up after each test method."""
        pass
    
    def test_addition(self):
        self.assertEqual(2 + 2, 4)
        self.assertNotEqual(2 + 2, 5)
    
    def test_division(self):
        with self.assertRaises(ZeroDivisionError):
            10 / 0
    
    def test_list_operations(self):
        self.assertIn(3, self.numbers)
        self.assertNotIn(6, self.numbers)
        self.assertEqual(len(self.numbers), 5)
    
    def test_float_comparison(self):
        self.assertAlmostEqual(0.1 + 0.2, 0.3, places=7)
    
    @unittest.skip("Temporarily disabled")
    def test_skipped(self):
        pass
    
    @unittest.skipIf(sys.platform == "win32", "Not supported on Windows")
    def test_platform_specific(self):
        pass

if __name__ == "__main__":
    unittest.main()

# pytest (popular alternative)
# pip install pytest

def test_addition():
    assert 2 + 2 == 4

def test_division_by_zero():
    import pytest
    with pytest.raises(ZeroDivisionError):
        10 / 0

# Fixtures
import pytest

@pytest.fixture
def sample_data():
    return [1, 2, 3, 4, 5]

def test_sum(sample_data):
    assert sum(sample_data) == 15

# Parametrized tests
@pytest.mark.parametrize("input,expected", [
    (2, 4),
    (3, 9),
    (4, 16),
])
def test_square(input, expected):
    assert input ** 2 == expected

# Mocking
from unittest.mock import Mock, patch

def test_with_mock():
    mock_obj = Mock()
    mock_obj.method.return_value = 42
    assert mock_obj.method() == 42

@patch('requests.get')
def test_api_call(mock_get):
    mock_get.return_value.status_code = 200
    mock_get.return_value.json.return_value = {"key": "value"}
    
    # Your code that uses requests.get
    response = requests.get("http://example.com")
    assert response.status_code == 200
```

## Debugging

📖 [Debugging Documentation](https://docs.python.org/3/library/pdb.html)

```python
# Using print for debugging
def problematic_function(x):
    print(f"Debug: x = {x}")  # Simple debug print
    result = x * 2
    print(f"Debug: result = {result}")
    return result

# Python debugger (pdb)
import pdb

def debug_me():
    x = 10
    pdb.set_trace()  # Debugger will stop here
    y = x * 2
    return y

# Breakpoint (Python 3.7+)
def debug_me_modern():
    x = 10
    breakpoint()  # Modern way to set breakpoint
    y = x * 2
    return y

# Logging for debugging
import logging

# Configure logging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

def calculate_average(numbers):
    logging.debug(f"Input numbers: {numbers}")
    if not numbers:
        logging.warning("Empty list provided")
        return 0
    
    total = sum(numbers)
    logging.debug(f"Sum: {total}")
    
    average = total / len(numbers)
    logging.info(f"Calculated average: {average}")
    return average

# Exception information
import traceback
import sys

try:
    problematic_code()
except Exception:
    # Print full traceback
    traceback.print_exc()
    
    # Get exception info
    exc_type, exc_value, exc_traceback = sys.exc_info()
    print(f"Exception type: {exc_type}")
    print(f"Exception value: {exc_value}")

# Assertions for debugging
def process_positive_number(num):
    assert num > 0, f"Number must be positive, got {num}"
    return num ** 2

# Debugging with rich (third-party library)
# pip install rich
from rich.console import Console
from rich.traceback import install

install()  # Beautiful tracebacks
console = Console()

def debug_with_rich():
    console.print("Debug message", style="bold red")
    console.print({"key": "value"})  # Pretty print objects
```

## File Operations

📖 [File I/O Documentation](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files)

```python
# Basic file operations
# Reading files
with open('file.txt', 'r') as f:
    content = f.read()          # Read entire file
    
with open('file.txt', 'r') as f:
    lines = f.readlines()       # Read all lines as list
    
with open('file.txt', 'r') as f:
    for line in f:              # Read line by line (memory efficient)
        print(line.strip())

# Writing files
with open('output.txt', 'w') as f:
    f.write('Hello, World!')
    
with open('output.txt', 'w') as f:
    lines = ['Line 1\n', 'Line 2\n', 'Line 3\n']
    f.writelines(lines)

# Appending to files
with open('log.txt', 'a') as f:
    f.write('New log entry\n')

# Binary files
with open('image.jpg', 'rb') as f:
    binary_data = f.read()

with open('copy.jpg', 'wb') as f:
    f.write(binary_data)

# File modes
# 'r' - read (default)
# 'w' - write (overwrites existing)
# 'a' - append
# 'x' - create (fails if exists)
# 'b' - binary mode
# 't' - text mode (default)
# '+' - update (read and write)

# Path operations
import os
from pathlib import Path

# Using os.path (traditional)
file_path = os.path.join('folder', 'subfolder', 'file.txt')
directory = os.path.dirname(file_path)
filename = os.path.basename(file_path)
exists = os.path.exists(file_path)

# Using pathlib (modern, recommended)
path = Path('folder') / 'subfolder' / 'file.txt'
directory = path.parent
filename = path.name
stem = path.stem          # filename without extension
suffix = path.suffix      # file extension
exists = path.exists()

# Working with directories
import os
import shutil

# Create directories
os.mkdir('new_folder')                    # Single directory
os.makedirs('path/to/nested/folder')      # Nested directories
Path('new/nested/folder').mkdir(parents=True, exist_ok=True)

# List directory contents
files = os.listdir('.')
files = list(Path('.').iterdir())

# Find files
txt_files = list(Path('.').glob('*.txt'))
all_files = list(Path('.').rglob('*'))    # Recursive

# Copy and move files
shutil.copy('source.txt', 'destination.txt')
shutil.move('old_location.txt', 'new_location.txt')
shutil.rmtree('directory_to_remove')

# File information
import stat
from datetime import datetime

file_stat = Path('file.txt').stat()
size = file_stat.st_size
modified_time = datetime.fromtimestamp(file_stat.st_mtime)
permissions = stat.filemode(file_stat.st_mode)
```

## Working with JSON

📖 [JSON Documentation](https://docs.python.org/3/library/json.html)

```python
import json

# Python object to JSON
data = {
    "name": "Alice",
    "age": 30,
    "active": True,
    "scores": [85, 92, 78],
    "address": {
        "street": "123 Main St",
        "city": "New York"
    }
}

# Convert to JSON string
json_string = json.dumps(data)
json_formatted = json.dumps(data, indent=2)
json_sorted = json.dumps(data, sort_keys=True, indent=2)

# Write to JSON file
with open('data.json', 'w') as f:
    json.dump(data, f, indent=2)

# JSON to Python object
json_string = '{"name": "Bob", "age": 25}'
parsed_data = json.loads(json_string)

# Read from JSON file
with open('data.json', 'r') as f:
    loaded_data = json.load(f)

# Custom JSON encoder
class DateTimeEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

# Usage with custom encoder
from datetime import datetime
data_with_date = {
    "event": "Meeting",
    "timestamp": datetime.now()
}
json_string = json.dumps(data_with_date, cls=DateTimeEncoder)

# Custom decoder
def datetime_decoder(dct):
    for key, value in dct.items():
        if key.endswith('_time') and isinstance(value, str):
            try:
                dct[key] = datetime.fromisoformat(value)
            except ValueError:
                pass
    return dct

parsed = json.loads(json_string, object_hook=datetime_decoder)

# Working with JSONLines (one JSON object per line)
import jsonlines

# Write JSONLines
data_list = [
    {"id": 1, "name": "Alice"},
    {"id": 2, "name": "Bob"},
    {"id": 3, "name": "Charlie"}
]

with jsonlines.open('data.jsonl', mode='w') as writer:
    for item in data_list:
        writer.write(item)

# Read JSONLines
with jsonlines.open('data.jsonl') as reader:
    for obj in reader:
        print(obj)
```

## Regular Expressions

📖 [Regular Expressions Documentation](https://docs.python.org/3/library/re.html)

```python
import re

# Basic patterns
text = "The phone numbers are 123-456-7890 and 987-654-3210"

# Search for pattern
match = re.search(r'\d{3}-\d{3}-\d{4}', text)
if match:
    print(match.group())  # 123-456-7890

# Find all matches
phone_numbers = re.findall(r'\d{3}-\d{3}-\d{4}', text)
print(phone_numbers)  # ['123-456-7890', '987-654-3210']

# Pattern replacement
new_text = re.sub(r'\d{3}-\d{3}-\d{4}', '[REDACTED]', text)
print(new_text)  # The phone numbers are [REDACTED] and [REDACTED]

# Compiled patterns (for repeated use)
phone_pattern = re.compile(r'\d{3}-\d{3}-\d{4}')
matches = phone_pattern.findall(text)

# Groups and capturing
email_text = "Contact us at info@example.com or support@test.org"
email_pattern = r'(\w+)@(\w+\.\w+)'
matches = re.findall(email_pattern, email_text)
print(matches)  # [('info', 'example.com'), ('support', 'test.org')]

# Named groups
pattern = r'(?P<username>\w+)@(?P<domain>\w+\.\w+)'
match = re.search(pattern, "user@example.com")
if match:
    print(match.group('username'))  # user
    print(match.group('domain'))    # example.com
    print(match.groupdict())        # {'username': 'user', 'domain': 'example.com'}

# Common patterns
patterns = {
    'email': r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b',
    'phone': r'\b\d{3}-\d{3}-\d{4}\b',
    'url': r'https?://(?:[-\w.])+(?:\:[0-9]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:\#(?:[\w.])*)?)?',
    'ip_address': r'\b(?:\d{1,3}\.){3}\d{1,3}\b',
    'credit_card': r'\b\d{4}[- ]?\d{4}[- ]?\d{4}[- ]?\d{4}\b',
    'date_mmddyyyy': r'\b\d{1,2}/\d{1,2}/\d{4}\b',
    'time_24h': r'\b\d{1,2}:\d{2}(?::\d{2})?\b'
}

# Validation function
def validate_email(email):
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
    
    return bool(re.match(pattern, email))

# Split using regex
text = "apple,banana;orange:grape"
fruits = re.split(r'[,;:]', text)
print(fruits)  # ['apple', 'banana', 'orange', 'grape']

# Flags
text = "Hello World"
matches = re.findall(r'hello', text, re.IGNORECASE)  # Case insensitive
multiline_text = "line1\nline2\nline3"
matches = re.findall(r'^line', multiline_text, re.MULTILINE)  # Multiline mode
```

## Standard Library

📖 [Standard Library Documentation](https://docs.python.org/3/library/index.html)

```python
# datetime
from datetime import datetime, date, time, timedelta

now = datetime.now()
today = date.today()
current_time = time()

# Date arithmetic
tomorrow = today + timedelta(days=1)
last_week = now - timedelta(weeks=1)

# Formatting
formatted = now.strftime("%Y-%m-%d %H:%M:%S")
parsed = datetime.strptime("2023-12-25", "%Y-%m-%d")

# collections
from collections import defaultdict, Counter, deque, namedtuple, OrderedDict

# defaultdict
dd = defaultdict(list)
dd['fruits'].append('apple')

# Counter
text = "hello world"
counter = Counter(text)
most_common = counter.most_common(3)

# deque (double-ended queue)
dq = deque([1, 2, 3])
dq.appendleft(0)  # Add to left
dq.append(4)      # Add to right

# itertools
import itertools

# Infinite iterators
count_iter = itertools.count(start=10, step=2)
cycle_iter = itertools.cycle(['A', 'B', 'C'])
repeat_iter = itertools.repeat('X', 5)

# Combinatorial iterators
combinations = list(itertools.combinations('ABCD', 2))
permutations = list(itertools.permutations('ABC', 2))
cartesian = list(itertools.product('AB', '12'))

# functools
from functools import reduce, partial, lru_cache, wraps

# reduce
numbers = [1, 2, 3, 4, 5]
total = reduce(lambda x, y: x + y, numbers)

# partial
def multiply(x, y):
    return x * y

double = partial(multiply, 2)
result = double(5)  # 10

# lru_cache
@lru_cache(maxsize=128)
def expensive_function(n):
    # Expensive computation
    return n ** 2

# random
import random

random.seed(42)           # Set seed for reproducibility
rand_int = random.randint(1, 10)
rand_float = random.random()
choice = random.choice(['a', 'b', 'c'])
sample = random.sample([1, 2, 3, 4, 5], 3)
random.shuffle(my_list)   # Shuffle in place

# os and sys
import os
import sys

# Environment variables
home = os.environ.get('HOME')
path = os.environ['PATH']

# System information
platform = sys.platform
version = sys.version
argv = sys.argv  # Command line arguments

# urllib (for HTTP requests)
from urllib.request import urlopen
from urllib.parse import urlencode, urlparse

response = urlopen('https://httpbin.org/get')
data = response.read()

# math
import math

result = math.sqrt(16)    # 4.0
result = math.ceil(4.2)   # 5
result = math.floor(4.8)  # 4
result = math.sin(math.pi / 2)  # 1.0
```

## Popular Third-Party Libraries

📖 [Python Package Index (PyPI)](https://pypi.org/)

```python
# requests - HTTP library
import requests

response = requests.get('https://api.github.com/users/octocat')
data = response.json()
status = response.status_code

# POST request
payload = {'key': 'value'}
response = requests.post('https://httpbin.org/post', json=payload)

# numpy - Numerical computing
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
matrix = np.array([[1, 2], [3, 4]])
zeros = np.zeros((3, 3))
ones = np.ones((2, 4))
random_arr = np.random.random((3, 3))

# Array operations
result = arr * 2
dot_product = np.dot(matrix, matrix)
transposed = matrix.T

# pandas - Data manipulation
import pandas as pd

# DataFrames
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'city': ['NY', 'LA', 'Chicago']
})

# Reading data
df = pd.read_csv('data.csv')
df = pd.read_excel('data.xlsx')
df = pd.read_json('data.json')

# Data operations
filtered = df[df['age'] > 25]
grouped = df.groupby('city')['age'].mean()
sorted_df = df.sort_values('age')

# matplotlib - Plotting
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

plt.plot(x, y)
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.title('Simple Plot')
plt.show()

# seaborn - Statistical plotting
import seaborn as sns

sns.scatterplot(data=df, x='age', y='salary')
sns.boxplot(data=df, x='department', y='salary')

# flask - Web framework
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

@app.route('/api/data', methods=['GET', 'POST'])
def handle_data():
    if request.method == 'POST':
        data = request.json
        return jsonify({'received': data})
    return jsonify({'message': 'Hello from API'})

if __name__ == '__main__':
    app.run(debug=True)

# SQLAlchemy - Database ORM
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    email = Column(String(100))

engine = create_engine('sqlite:///example.db')
Base.metadata.create_all(engine)
Session = sessionmaker(bind=engine)
```

## Code Style & PEP 8

📖 [PEP 8 Style Guide](https://peps.python.org/pep-0008/)

```python
# Naming conventions
class MyClass:           # CapitalizedWords (PascalCase)
    pass

def my_function():       # lowercase_with_underscores (snake_case)
    pass

CONSTANT_VALUE = 42      # UPPERCASE_WITH_UNDERSCORES

my_variable = "value"    # lowercase_with_underscores
_private_var = "private" # Leading underscore for internal use

# Indentation: 4 spaces per level
if True:
    print("Properly indented")
    if True:
        print("Nested indentation")

# Line length: Maximum 79 characters
long_variable_name = some_function(argument_one, argument_two,
                                   argument_three, argument_four)

# Alternative for long lines
long_variable_name = some_function(
    argument_one,
    argument_two,
    argument_three,
    argument_four
)

# Imports
import os
import sys
from collections import defaultdict, Counter

# Not recommended
import os, sys

# Blank lines
class MyClass:
    """Class docstring."""
    
    def __init__(self):
        """Constructor."""
        pass
    
    def method_one(self):
        """First method."""
        pass
    
    def method_two(self):
        """Second method."""
        pass


def standalone_function():
    """Standalone function."""
    pass

# Comments
# This is a good comment explaining what follows
x = x + 1  # Increment x

# Avoid obvious comments
x = x + 1  # Add 1 to x  # Bad comment

# Docstrings
def calculate_area(radius):
    """
    Calculate the area of a circle.
    
    Args:
        radius (float): The radius of the circle.
        
    Returns:
        float: The area of the circle.
        
    Raises:
        ValueError: If radius is negative.
    """
    if radius < 0:
        raise ValueError("Radius cannot be negative")
    return 3.14159 * radius ** 2

# String quotes: Be consistent
single_quoted = 'Hello'
double_quoted = "World"
triple_quoted = """Multi-line
string"""

# Whitespace in expressions
# Good
x = y + z
list_item = my_list[index]
result = function(arg1, arg2)

# Bad
x=y+z
list_item = my_list[ index ]
result = function( arg1 , arg2 )
```

## Performance Tips

📖 [Performance Tips Documentation](https://wiki.python.org/moin/PythonSpeed/PerformanceTips)

```python
# Use list comprehensions instead of loops
# Good
squares = [x**2 for x in range(1000)]

# Less efficient
squares = []
for x in range(1000):
    squares.append(x**2)

# Use built-in functions
# Good
total = sum(numbers)
maximum = max(numbers)

# Less efficient
total = 0
for num in numbers:
    total += num

# Use generators for large datasets
def fibonacci_generator():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Use appropriate data structures
# For membership testing, use sets
large_set = set(range(10000))
if 5000 in large_set:  # O(1) average case
    pass

large_list = list(range(10000))
if 5000 in large_list:  # O(n)
    pass

# Use local variables in loops
import math

# Good
def process_numbers(numbers):
    sqrt = math.sqrt  # Local reference
    return [sqrt(x) for x in numbers]

# Less efficient (global lookup each time)
def process_numbers(numbers):
    return [math.sqrt(x) for x in numbers]

# Use string methods instead of regex for simple operations
# Good
if text.startswith('prefix'):
    pass

# Less efficient for simple cases
import re
if re.match(r'^prefix', text):
    pass

# Cache expensive function calls
from functools import lru_cache

@lru_cache(maxsize=128)
def expensive_function(n):
    # Expensive computation
    return result

# Use slots for classes with many instances
class Point:
    __slots__ = ['x', 'y']
    
    def __init__(self, x, y):
        self.x = x
        self.y = y

# Profile your code
import cProfile
import pstats

def main():
    # Your code here
    pass

if __name__ == '__main__':
    cProfile.run('main()', 'profile_stats')
    stats = pstats.Stats('profile_stats')
    stats.sort_stats('cumulative').print_stats(10)

# Use timeit for micro-benchmarks
import timeit

time_taken = timeit.timeit(
    'x = [i**2 for i in range(100)]',
    number=10000
)
print(f"Time taken: {time_taken:.6f} seconds")
```

## Common Patterns

📖 [Design Patterns in Python](https://python-patterns.guide/)

```python
# Singleton pattern
class Singleton:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

# Factory pattern
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

class AnimalFactory:
    @staticmethod
    def create_animal(animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        else:
            raise ValueError("Unknown animal type")

# Observer pattern
class Observable:
    def __init__(self):
        self._observers = []
    
    def attach(self, observer):
        self._observers.append(observer)
    
    
    