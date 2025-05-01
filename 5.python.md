# Complete Python Fundamentals Guide

## Table of Contents
1. [Introduction to Python](#introduction-to-python)
2. [Basic Syntax](#basic-syntax)
3. [Variables and Data Types](#variables-and-data-types)
4. [Operators](#operators)
5. [Control Flow](#control-flow)
6. [Functions](#functions)
7. [Data Structures](#data-structures)
8. [File Handling](#file-handling)
9. [Exception Handling](#exception-handling)
10. [Modules and Packages](#modules-and-packages)
11. [Object-Oriented Programming (OOP)](#object-oriented-programming-oop)
12. [Standard Library](#standard-library)
13. [Advanced Concepts](#advanced-concepts)
14. [Python Development Tools](#python-development-tools)
15. [Best Practices](#best-practices)

## Introduction to Python

Python is a high-level, interpreted programming language known for its readability and simplicity. Created by Guido van Rossum and first released in 1991, Python emphasizes code readability with its use of significant whitespace.

### Key Features:
- **Easy to learn and use**: Simple syntax that's readable and expressive
- **Interpreted**: No compilation needed
- **Dynamically typed**: No need to declare variable types
- **Garbage collected**: Automatic memory management
- **Multi-paradigm**: Supports procedural, object-oriented, and functional programming
- **Extensive libraries**: Rich ecosystem of third-party packages

```python
# Your first Python program
print("Hello, World!")  # This prints "Hello, World!" to the console
```

## Basic Syntax

Python's syntax is designed to be readable and straightforward.

### Indentation
Python uses indentation (whitespace) to define code blocks, rather than curly braces or keywords.

```python
# Indentation example
if True:
    print("This is indented")  # 4 spaces is the standard indentation
    if True:
        print("Further indented")  # Another level of indentation
print("Not indented")  # Back to the original indentation level
```

### Comments
Comments are used to explain code and are ignored by the interpreter.

```python
# This is a single-line comment

"""
This is a multi-line comment
or docstring (when used at the beginning of a function, class, or module)
"""
```

### Line Continuation
Long lines can be broken up for readability.

```python
# Line continuation example
long_string = "This is a very long string that " + \
              "continues on the next line"  # Using backslash

# Or using parentheses (preferred)
sum_result = (10 + 20 + 30 +
              40 + 50)
```

## Variables and Data Types

### Variables
In Python, variables are created when you assign a value to them. No declaration is needed.

```python
# Variable assignment
name = "John"  # String variable
age = 30       # Integer variable
height = 5.9   # Float variable
is_student = True  # Boolean variable

# Multiple assignment
x, y, z = 1, 2, 3

# Variable naming rules:
# - Must start with a letter or underscore
# - Can contain letters, numbers, and underscores
# - Case-sensitive (age is different from Age)
```

### Basic Data Types

#### Numbers
```python
# Integers
x = 10
y = -5
big_number = 1_000_000  # Underscores for readability

# Floating point numbers
pi = 3.14159
scientific = 1.23e-4  # Scientific notation

# Complex numbers
complex_num = 3 + 4j
```

#### Strings
```python
# String creation
single_quoted = 'Hello'
double_quoted = "World"
triple_quoted = '''Multiple
lines'''

# String operations
greeting = single_quoted + " " + double_quoted  # Concatenation
repeated = "Echo " * 3  # Repetition: "Echo Echo Echo "

# String methods
name = "John Doe"
print(name.upper())  # Convert to uppercase: "JOHN DOE"
print(name.lower())  # Convert to lowercase: "john doe"
print(name.split())  # Split by whitespace: ["John", "Doe"]

# String formatting
name = "Alice"
age = 25
# f-strings (Python 3.6+)
print(f"{name} is {age} years old")
# str.format()
print("{} is {} years old".format(name, age))
# %-formatting (older style)
print("%s is %d years old" % (name, age))

# String indexing and slicing
message = "Python"
print(message[0])    # First character: "P"
print(message[-1])   # Last character: "n"
print(message[0:2])  # First two characters: "Py"
print(message[2:])   # From third character to end: "thon"
print(message[:3])   # First three characters: "Pyt"
```

#### Booleans
```python
# Boolean values
is_valid = True
is_completed = False

# Boolean operations
print(is_valid and is_completed)  # Logical AND: False
print(is_valid or is_completed)   # Logical OR: True
print(not is_valid)               # Logical NOT: False
```

#### None
```python
# None type represents absence of value
result = None
print(result is None)  # True
```

## Operators

### Arithmetic Operators
```python
a, b = 10, 3

# Basic arithmetic
print(a + b)  # Addition: 13
print(a - b)  # Subtraction: 7
print(a * b)  # Multiplication: 30
print(a / b)  # Division: 3.3333... (returns float)
print(a // b) # Floor division: 3 (returns integer)
print(a % b)  # Modulus (remainder): 1
print(a ** b) # Exponentiation: 1000 (10^3)
```

### Comparison Operators
```python
a, b = 10, 3

print(a == b)  # Equal to: False
print(a != b)  # Not equal to: True
print(a > b)   # Greater than: True
print(a < b)   # Less than: False
print(a >= b)  # Greater than or equal to: True
print(a <= b)  # Less than or equal to: False
```

### Assignment Operators
```python
a = 10  # Simple assignment

# Compound assignment operators
a += 5   # Same as: a = a + 5
a -= 2   # Same as: a = a - 2
a *= 3   # Same as: a = a * 3
a /= 2   # Same as: a = a / 2
a //= 2  # Same as: a = a // 2
a %= 3   # Same as: a = a % 3
a **= 2  # Same as: a = a ** 2
```

### Bitwise Operators
```python
a, b = 10, 3  # In binary: a = 1010, b = 0011

print(a & b)   # Bitwise AND: 2 (0010)
print(a | b)   # Bitwise OR: 11 (1011)
print(a ^ b)   # Bitwise XOR: 9 (1001)
print(~a)      # Bitwise NOT: -11 (inverts all bits)
print(a << 1)  # Left shift: 20 (10100)
print(a >> 1)  # Right shift: 5 (0101)
```

### Identity Operators
```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a is c)      # True (a and c reference the same object)
print(a is b)      # False (a and b are different objects)
print(a is not b)  # True (a and b are different objects)
```

### Membership Operators
```python
numbers = [1, 2, 3, 4, 5]

print(3 in numbers)      # True (3 is in the list)
print(6 in numbers)      # False (6 is not in the list)
print(6 not in numbers)  # True (6 is not in the list)
```

## Control Flow

### Conditional Statements
```python
# if statement
age = 18
if age < 18:
    print("Minor")
elif age == 18:  # elif = else if
    print("Just became an adult")
else:
    print("Adult")

# Ternary operator (conditional expression)
status = "Adult" if age >= 18 else "Minor"
```

### Loops

#### For Loop
```python
# Iterating over a sequence
for i in range(5):  # range(5) creates sequence 0, 1, 2, 3, 4
    print(i)

# Iterating over a list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Enumerate for index and value
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# Loop with else (executed when loop completes normally)
for i in range(3):
    print(i)
else:
    print("Loop completed")
```

#### While Loop
```python
# Basic while loop
count = 0
while count < 5:
    print(count)
    count += 1

# While loop with else
count = 0
while count < 3:
    print(count)
    count += 1
else:
    print("While loop completed")
```

### Loop Control
```python
# break statement
for i in range(10):
    if i == 5:
        break  # Exit the loop
    print(i)  # Prints 0, 1, 2, 3, 4

# continue statement
for i in range(5):
    if i == 2:
        continue  # Skip the rest of the current iteration
    print(i)  # Prints 0, 1, 3, 4

# pass statement (does nothing, placeholder)
for i in range(3):
    if i == 1:
        pass  # Placeholder for future code
    print(i)  # Prints 0, 1, 2
```

## Functions

### Basic Function
```python
# Defining a function
def greet(name):
    """This is a docstring: describes what the function does"""
    return f"Hello, {name}!"

# Calling a function
message = greet("Alice")
print(message)  # Output: Hello, Alice!
```

### Parameters and Arguments
```python
# Default parameter values
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet("Bob"))          # Output: Hello, Bob!
print(greet("Bob", "Hi"))    # Output: Hi, Bob!

# Keyword arguments
def describe_person(name, age, job):
    return f"{name} is {age} years old and works as a {job}."

# Positional arguments
print(describe_person("Alice", 30, "developer"))
# Keyword arguments (order doesn't matter)
print(describe_person(age=30, job="developer", name="Alice"))
# Mix of positional and keyword arguments
print(describe_person("Alice", job="developer", age=30))
```

### Variable Number of Arguments
```python
# *args for variable number of positional arguments
def sum_all(*numbers):
    """Sum all numbers passed to the function"""
    total = 0
    for num in numbers:
        total += num
    return total

print(sum_all(1, 2, 3, 4))  # Output: 10

# **kwargs for variable number of keyword arguments
def print_info(**kwargs):
    """Print all keyword arguments"""
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=30, job="developer")
```

### Lambda Functions (Anonymous Functions)
```python
# Lambda function syntax: lambda arguments: expression
square = lambda x: x ** 2
print(square(5))  # Output: 25

# Common use with built-in functions
numbers = [1, 5, 3, 9, 2]
sorted_numbers = sorted(numbers, key=lambda x: x)
print(sorted_numbers)  # Output: [1, 2, 3, 5, 9]
```

### Function Scope
```python
# Variable scope example
x = 10  # Global variable

def func1():
    y = 5  # Local variable
    print(x)  # Can access global variable
    print(y)  # Can access local variable

def func2():
    # print(y)  # Would raise an error - y is not defined here
    print(x)  # Can access global variable

# Modifying global variables
def func3():
    global x  # Declare x as global
    x = 20    # Modify global x

print(x)      # Output: 10
func3()
print(x)      # Output: 20
```

### Recursion
```python
# Factorial function using recursion
def factorial(n):
    """Calculate factorial of n recursively"""
    if n <= 1:  # Base case
        return 1
    else:       # Recursive case
        return n * factorial(n - 1)

print(factorial(5))  # 5! = 5 * 4 * 3 * 2 * 1 = 120
```

## Data Structures

### Lists
```python
# Creating lists
empty_list = []
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]

# Accessing elements
print(numbers[0])    # First element: 1
print(numbers[-1])   # Last element: 5

# Slicing
print(numbers[1:3])  # Elements at index 1 and 2: [2, 3]

# List methods
numbers.append(6)        # Add an element to the end
numbers.insert(0, 0)     # Insert 0 at position 0
numbers.remove(3)        # Remove first occurrence of 3
popped = numbers.pop()   # Remove and return last element
numbers.sort()           # Sort the list in-place
numbers.reverse()        # Reverse the list in-place
count = numbers.count(2) # Count occurrences of 2
length = len(numbers)    # Get list length

# List operations
combined = numbers + [7, 8, 9]  # Concatenation
repeated = [0] * 3              # Repetition: [0, 0, 0]

# List comprehensions
squares = [x**2 for x in range(5)]  # [0, 1, 4, 9, 16]
even_squares = [x**2 for x in range(10) if x % 2 == 0]  # [0, 4, 16, 36, 64]
```

### Tuples
```python
# Creating tuples
empty_tuple = ()
single_item = (1,)  # Note the comma
coordinates = (10, 20)
mixed = (1, "hello", 3.14)

# Accessing elements (same as lists)
print(coordinates[0])  # First element: 10

# Tuple methods (fewer than lists because tuples are immutable)
count = mixed.count("hello")  # Count occurrences of "hello"
index = mixed.index(3.14)     # Get index of 3.14

# Tuple packing and unpacking
person = ("John", 30, "developer")  # Packing
name, age, job = person             # Unpacking

# Tuples are immutable
# coordinates[0] = 15  # This would raise an error
```

### Dictionaries
```python
# Creating dictionaries
empty_dict = {}
person = {
    "name": "John",
    "age": 30,
    "job": "developer"
}

# Accessing elements
print(person["name"])  # Access by key: "John"
print(person.get("age"))  # Using get method: 30
print(person.get("salary", "Not specified"))  # With default value

# Dictionary methods
keys = person.keys()      # Get all keys
values = person.values()  # Get all values
items = person.items()    # Get all key-value pairs as tuples

person["salary"] = 50000  # Add new key-value pair
person.update({"age": 31, "location": "New York"})  # Update multiple keys
del person["job"]         # Remove a key-value pair
popped = person.pop("age")  # Remove and return a value

# Dictionary comprehensions
squares_dict = {x: x**2 for x in range(5)}  # {0:0, 1:1, 2:4, 3:9, 4:16}
```

### Sets
```python
# Creating sets
empty_set = set()  # Note: {} creates an empty dictionary, not a set
fruits = {"apple", "banana", "cherry"}
numbers = {1, 2, 3, 3, 4}  # Duplicates are removed: {1, 2, 3, 4}

# Set methods
fruits.add("orange")       # Add an element
fruits.remove("banana")    # Remove an element (raises error if not found)
fruits.discard("grape")    # Remove if present (no error if not found)
popped = fruits.pop()      # Remove and return an arbitrary element
fruits.clear()             # Remove all elements

# Set operations
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

union = set1 | set2              # Union: {1, 2, 3, 4, 5, 6}
intersection = set1 & set2       # Intersection: {3, 4}
difference = set1 - set2         # Difference: {1, 2}
symmetric_difference = set1 ^ set2  # Symmetric difference: {1, 2, 5, 6}

# Set comprehensions
even_set = {x for x in range(10) if x % 2 == 0}  # {0, 2, 4, 6, 8}
```

## File Handling

### Reading Files
```python
# Open a file for reading
with open("example.txt", "r") as file:
    # Read the entire file as a string
    content = file.read()
    print(content)

# Reading line by line
with open("example.txt", "r") as file:
    for line in file:
        print(line.strip())  # strip() removes trailing newline

# Read specific number of characters
with open("example.txt", "r") as file:
    first_10 = file.read(10)  # Read first 10 characters
    print(first_10)

# Read all lines into a list
with open("example.txt", "r") as file:
    lines = file.readlines()
    print(lines)
```

### Writing Files
```python
# Write to a file (overwrites existing content)
with open("output.txt", "w") as file:
    file.write("Hello, world!\n")
    file.write("This is a new line.")

# Append to a file
with open("output.txt", "a") as file:
    file.write("\nThis line is appended.")

# Write multiple lines at once
lines = ["Line 1", "Line 2", "Line 3"]
with open("output.txt", "w") as file:
    file.writelines(line + "\n" for line in lines)
```

### Working with CSV Files
```python
import csv

# Reading a CSV file
with open("data.csv", "r") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)  # Each row is a list of values

# Reading CSV with headers
with open("data.csv", "r") as file:
    reader = csv.DictReader(file)  # Uses first row as field names
    for row in reader:
        print(row)  # Each row is a dictionary

# Writing a CSV file
data = [
    ["Name", "Age", "Job"],
    ["Alice", 30, "Developer"],
    ["Bob", 25, "Designer"]
]
with open("output.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerows(data)

# Writing CSV with dictionaries
dict_data = [
    {"Name": "Alice", "Age": 30, "Job": "Developer"},
    {"Name": "Bob", "Age": 25, "Job": "Designer"}
]
with open("output.csv", "w", newline="") as file:
    fieldnames = ["Name", "Age", "Job"]
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(dict_data)
```

### Working with JSON Files
```python
import json

# Reading JSON
with open("data.json", "r") as file:
    data = json.load(file)  # Parse JSON into Python objects
    print(data)

# Writing JSON
data = {
    "name": "Alice",
    "age": 30,
    "skills": ["Python", "JavaScript", "SQL"]
}
with open("output.json", "w") as file:
    json.dump(data, file, indent=4)  # indent for pretty formatting

# Converting Python objects to JSON string
json_string = json.dumps(data, indent=4)
print(json_string)

# Parsing JSON string
parsed_data = json.loads('{"name": "Bob", "age": 25}')
print(parsed_data)
```

## Exception Handling

### Basic Exception Handling
```python
# Try-except block
try:
    x = 10 / 0  # This will raise a ZeroDivisionError
except ZeroDivisionError:
    print("Cannot divide by zero!")

# Handling multiple exceptions
try:
    num = int("abc")  # This will raise a ValueError
except ValueError:
    print("Invalid number!")
except ZeroDivisionError:
    print("Cannot divide by zero!")

# Catching any exception
try:
    # Some risky operation
    with open("nonexistent.txt", "r") as file:
        content = file.read()
except Exception as e:
    print(f"An error occurred: {e}")
```

### Try-Except-Else-Finally
```python
try:
    num = int(input("Enter a number: "))
    result = 100 / num
except ValueError:
    print("That's not a valid number!")
except ZeroDivisionError:
    print("Cannot divide by zero!")
else:
    # Executed if no exceptions were raised
    print(f"Result: {result}")
finally:
    # Always executed, regardless of exceptions
    print("End of calculation")
```

### Raising Exceptions
```python
def validate_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 120:
        raise ValueError("Age is too high")
    return age

try:
    validate_age(-5)
except ValueError as e:
    print(e)  # Output: Age cannot be negative
```

### Custom Exceptions
```python
# Creating a custom exception
class InsufficientFundsError(Exception):
    """Raised when a withdrawal exceeds available balance"""
    pass

def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientFundsError("Not enough funds in account")
    return balance - amount

try:
    withdraw(100, 150)
except InsufficientFundsError as e:
    print(e)  # Output: Not enough funds in account
```

## Modules and Packages

### Importing Modules
```python
# Importing an entire module
import math
print(math.sqrt(16))  # Output: 4.0

# Importing specific functions
from math import sqrt, pi
print(sqrt(16))  # Output: 4.0
print(pi)        # Output: 3.141592...

# Importing with alias
import math as m
print(m.sqrt(16))  # Output: 4.0

# Importing all functions (not recommended)
from math import *
print(sqrt(16))  # Output: 4.0
```

### Creating Modules
```python
# File: mymodule.py
def greet(name):
    return f"Hello, {name}!"

def add(a, b):
    return a + b

PI = 3.14159

# File: main.py
import mymodule

print(mymodule.greet("Alice"))  # Output: Hello, Alice!
print(mymodule.add(5, 3))       # Output: 8
print(mymodule.PI)              # Output: 3.14159
```

### Packages
A package is a directory containing multiple modules and a special `__init__.py` file.

```
mypackage/
    __init__.py
    module1.py
    module2.py
    subpackage/
        __init__.py
        module3.py
```

```python
# Importing from a package
import mypackage.module1
from mypackage import module2
from mypackage.subpackage import module3
from mypackage.module1 import function1
```

### Standard Library Modules
```python
# Math operations
import math
print(math.sqrt(16))  # Square root
print(math.sin(math.pi/2))  # Trigonometric functions

# Random numbers
import random
print(random.randint(1, 10))  # Random integer between 1 and 10
print(random.choice(["apple", "banana", "cherry"]))  # Random item from list

# Date and time
import datetime
now = datetime.datetime.now()
print(now)
print(now.strftime("%Y-%m-%d %H:%M:%S"))  # Format date

# Operating system interface
import os
print(os.getcwd())  # Current working directory
os.mkdir("new_directory")  # Create directory

# System-specific parameters and functions
import sys
print(sys.version)  # Python version
print(sys.platform)  # Operating system platform

# Regular expressions
import re
text = "The phone number is 123-456-7890"
match = re.search(r"\d{3}-\d{3}-\d{4}", text)
print(match.group())  # Output: 123-456-7890
```

## Object-Oriented Programming (OOP)

### Classes and Objects
```python
# Class definition
class Person:
    # Class variable (shared by all instances)
    species = "Human"
    
    # Constructor method
    def __init__(self, name, age):
        # Instance variables (unique to each instance)
        self.name = name
        self.age = age
    
    # Instance method
    def greet(self):
        return f"Hello, my name is {self.name}"
    
    # Instance method with parameters
    def celebrate_birthday(self):
        self.age += 1
        return f"{self.name} is now {self.age} years old"

# Creating objects (instances)
person1 = Person("Alice", 30)
person2 = Person("Bob", 25)

# Accessing attributes
print(person1.name)  # Output: Alice
print(person2.age)   # Output: 25
print(Person.species)  # Accessing class variable: Human

# Calling methods
print(person1.greet())  # Output: Hello, my name is Alice
print(person2.celebrate_birthday())  # Output: Bob is now 26 years old
```

### Class Methods and Static Methods
```python
class MathUtils:
    # Class variable
    pi = 3.14159
    
    # Instance method (requires an instance)
    def calculate_area(self, radius):
        return self.pi * radius ** 2
    
    # Class method (works with the class, not instances)
    @classmethod
    def from_diameter(cls, diameter):
        radius = diameter / 2
        return cls(radius)  # Creates a new instance
    
    # Static method (doesn't need class or instance)
    @staticmethod
    def add(a, b):
        return a + b

# Using class methods and static methods
print(MathUtils.pi)  # Accessing class variable
print(MathUtils.add(5, 3))  # Calling static method

# Creating an instance
math = MathUtils()
print(math.calculate_area(5))  # Calling instance method
```

### Inheritance
```python
# Base class (parent)
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"

# Derived class (child)
class Dog(Animal):
    def __init__(self, name, breed):
        # Call parent class's __init__
        super().__init__(name)
        self.breed = breed
    
    # Override parent's method
    def speak(self):
        return "Woof!"
    
    # New method in child class
    def fetch(self):
        return f"{self.name} is fetching the ball"

# Another derived class
class Cat(Animal):
    def speak(self):
        return "Meow!"

# Creating objects
dog = Dog("Rex", "Golden Retriever")
cat = Cat("Whiskers")

print(dog.name)    # Inherited attribute: Rex
print(dog.breed)   # New attribute: Golden Retriever
print(dog.speak()) # Overridden method: Woof!
print(cat.speak()) # Overridden method: Meow!
print(dog.fetch()) # New method: Rex is fetching the ball
```

### Multiple Inheritance
```python
class Swimming:
    def swim(self):
        return "Swimming"

class Flying:
    def fly(self):
        return "Flying"

# Multiple inheritance
class Duck(Swimming, Flying):
    def speak(self):
        return "Quack!"

duck = Duck()
print(duck.swim())  # From Swimming class: Swimming
print(duck.fly())   # From Flying class: Flying
print(duck.speak()) # Own method: Quack!
```

### Encapsulation
```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner       # Public attribute
        self.__balance = balance  # Private attribute (name mangling)
    
    # Getter method
    def get_balance(self):
        return self.__balance
    
    # Setter method
    def set_balance(self, amount):
        if amount >= 0:
            self.__balance = amount
        else:
            print("Balance cannot be negative")
    
    # Public method
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return f"Deposited ${amount}. New balance: ${self.__balance}"
        return "Invalid deposit amount"
    
    # Public method
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return f"Withdrew ${amount}. New balance: ${self.__balance}"
        return "Insufficient funds or invalid amount"

# Creating an account
account = BankAccount("Alice", 1000)

# Using public methods to access private attribute
print(account.get_balance())  # Output: 1000
account.set_balance(1500)
print(account.get_balance())  # Output: 1500

# Using encapsulated operations
print(account.deposit(500))   # Output: Deposited $500. New balance: $2000
print(account.withdraw(800))  # Output: Withdrew $800. New balance: $1200

# Direct access to private attribute is not recommended
# print(account.__balance)  # Would raise an AttributeError
# Python's name mangling allows access via _BankAccount__balance
print(account._BankAccount__balance)  # Output: 1200 (but not recommended)
```

### Property Decorators
```python
class Person:
    def __init__(self, name, age):
        self._name = name
        self._age = age
    
    # Getter property
    @property
    def age(self):
        return self._age
    
    # Setter property
    @age.setter
    def age(self, value):
        if value >= 0:
            self._age = value
        else:
            raise ValueError("Age cannot be negative")
    
    # Property with only a getter (read-only)
    @property
    def name(self):
        return self._name

# Using properties
person = Person("Alice", 30)
print(person.name)  # Using the getter: Alice
print(person.age)   # Using the getter: 30

person.age = 31     # Using the setter
print(person.age)   # 31

# person.name = "Bob"  # Would raise an AttributeError (no setter defined)
# person.age = -5     # Would raise a ValueError (age validation)
```

### Polymorphism
```python
class Animal:
    def speak(self):
        pass  # Abstract method to be overridden

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

class Duck(Animal):
    def speak(self):
        return "Quack!"

# Polymorphic function
def animal_sound(animal):
    return animal.speak()

# Using polymorphism
dog = Dog()
cat = Cat()
duck = Duck()

print(animal_sound(dog))  # Output: Woof!
print(animal_sound(cat))  # Output: Meow!
print(animal_sound(duck)) # Output: Quack!
```

### Magic Methods (Dunder Methods)
```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # String representation
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    # Representation (for debugging)
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"
    
    # Addition
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    # Subtraction
    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)
    
    # Equality
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    # Length (magnitude)
    def __abs__(self):
        return (self.x**2 + self.y**2)**0.5
    
    # Make iterable
    def __iter__(self):
        return iter([self.x, self.y])

# Using magic methods
v1 = Vector(2, 3)
v2 = Vector(3, 4)

print(v1)             # Output: Vector(2, 3)
print(v1 + v2)        # Output: Vector(5, 7)
print(v1 - v2)        # Output: Vector(-1, -1)
print(v1 == Vector(2, 3))  # Output: True
print(abs(v1))        # Output: 3.605551275463989
print(list(v1))       # Output: [2, 3]
```

## Standard Library

### Working with Dates and Times
```python
from datetime import datetime, date, time, timedelta

# Current date and time
now = datetime.now()
print(now)  # Current datetime

# Creating date objects
today = date.today()
print(today)  # Current date
custom_date = date(2023, 1, 15)
print(custom_date)  # 2023-01-15

# Creating time objects
current_time = datetime.now().time()
print(current_time)  # Current time
custom_time = time(13, 30, 45)
print(custom_time)  # 13:30:45

# Date formatting
formatted_date = now.strftime("%Y-%m-%d %H:%M:%S")
print(formatted_date)  # YYYY-MM-DD HH:MM:SS

# Parsing string to date
date_string = "2023-01-15"
parsed_date = datetime.strptime(date_string, "%Y-%m-%d")
print(parsed_date)  # 2023-01-15 00:00:00

# Date arithmetic
tomorrow = today + timedelta(days=1)
print(tomorrow)
next_week = today + timedelta(weeks=1)
print(next_week)
two_hours_later = datetime.now() + timedelta(hours=2)
print(two_hours_later)
```

### Regular Expressions
```python
import re

# Basic pattern matching
text = "The phone number is 123-456-7890"
pattern = r"\d{3}-\d{3}-\d{4}"
match = re.search(pattern, text)
if match:
    print(match.group())  # Output: 123-456-7890

# Finding all matches
text = "Phone numbers: 123-456-7890, 987-654-3210"
matches = re.findall(pattern, text)
print(matches)  # Output: ['123-456-7890', '987-654-3210']

# Substitution
new_text = re.sub(pattern, "XXX-XXX-XXXX", text)
print(new_text)  # Output: Phone numbers: XXX-XXX-XXXX, XXX-XXX-XXXX

# Match object properties
text = "Python was created in 1991"
match = re.search(r"(\d{4})", text)
if match:
    print(match.group(1))  # Output: 1991
    print(match.start())   # Output: 20
    print(match.end())     # Output: 24

# Regex flags
text = "PYTHON is awesome"
match = re.search(r"python", text, re.IGNORECASE)
if match:
    print(match.group())  # Output: PYTHON
```

### Math and Statistics
```python
import math
import statistics

# Basic math functions
print(math.sqrt(16))  # Square root: 4.0
print(math.pow(2, 3))  # Power: 8.0
print(math.ceil(4.2))  # Ceiling: 5
print(math.floor(4.8))  # Floor: 4
print(math.factorial(5))  # Factorial: 120
print(math.gcd(12, 8))  # Greatest common divisor: 4

# Trigonometric functions
print(math.sin(math.pi/2))  # Sine: 1.0
print(math.cos(math.pi))    # Cosine: -1.0
print(math.tan(math.pi/4))  # Tangent: 1.0

# Constants
print(math.pi)  # Pi: 3.141592653589793
print(math.e)   # Euler's number: 2.718281828459045

# Statistics
data = [1, 2, 3, 4, 5, 5, 6, 7, 8]
print(statistics.mean(data))    # Mean: 4.555...
print(statistics.median(data))  # Median: 5
print(statistics.mode(data))    # Mode: 5
print(statistics.stdev(data))   # Standard deviation: 2.18...
```

### Random Numbers
```python
import random

# Random integers
print(random.randint(1, 10))  # Random integer between 1 and 10
print(random.randrange(0, 100, 5))  # Random multiple of 5 between 0 and 100

# Random floating point
print(random.random())  # Random float between 0 and 1
print(random.uniform(1.5, 3.5))  # Random float between 1.5 and 3.5

# Random selections
print(random.choice(["apple", "banana", "cherry"]))  # Random item from list
print(random.sample(range(1, 50), 6))  # 6 unique random numbers (lottery)

# Shuffling
deck = list(range(52))  # A deck of cards
random.shuffle(deck)    # Shuffle in-place
print(deck[:5])         # First 5 cards

# Setting seed for reproducibility
random.seed(42)
print(random.randint(1, 100))  # Always gives same number with same seed
```

### Collections
```python
from collections import Counter, defaultdict, namedtuple, deque, OrderedDict

# Counter: count occurrences of elements
fruits = ["apple", "banana", "apple", "orange", "banana", "apple"]
fruit_count = Counter(fruits)
print(fruit_count)  # Counter({'apple': 3, 'banana': 2, 'orange': 1})
print(fruit_count["apple"])  # 3
print(fruit_count.most_common(2))  # [('apple', 3), ('banana', 2)]

# defaultdict: dictionary with default factory
# Regular dictionaries raise KeyError for missing keys
scores = defaultdict(int)  # Default value of 0 for new keys
scores["Alice"] += 5
print(scores["Alice"])  # 5
print(scores["Bob"])    # 0 (default value, no KeyError)

# namedtuple: tuple with named fields
Person = namedtuple("Person", ["name", "age", "job"])
alice = Person("Alice", 30, "Developer")
print(alice.name)  # Alice
print(alice.age)   # 30
print(alice[2])    # Developer (can still use index)

# deque: double-ended queue
queue = deque(["one", "two", "three"])
queue.append("four")       # Add to right side
queue.appendleft("zero")   # Add to left side
print(queue)  # deque(['zero', 'one', 'two', 'three', 'four'])
print(queue.pop())        # Remove from right: 'four'
print(queue.popleft())    # Remove from left: 'zero'

# OrderedDict: dictionary that remembers insertion order
# (Note: in Python 3.7+, regular dict also preserves order)
ordered = OrderedDict()
ordered["a"] = 1
ordered["b"] = 2
ordered["c"] = 3
print(list(ordered.items()))  # [('a', 1), ('b', 2), ('c', 3)]
```

### Working with Paths
```python
import os
from pathlib import Path

# OS path operations
current_dir = os.getcwd()
print(current_dir)  # Current working directory

file_path = os.path.join(current_dir, "example.txt")
print(file_path)    # Proper path with OS-specific separators

print(os.path.exists(file_path))    # Check if file exists
print(os.path.isfile(file_path))    # Check if path is a file
print(os.path.isdir(current_dir))   # Check if path is a directory

# File operations
filename = os.path.basename(file_path)  # Get filename: example.txt
dirname = os.path.dirname(file_path)    # Get directory path
root, ext = os.path.splitext(filename)  # Split extension: ('example', '.txt')

# Using pathlib (more modern, object-oriented)
path = Path(current_dir) / "example.txt"
print(path)  # Full path to example.txt

print(path.exists())       # Check if file exists
print(path.is_file())      # Check if path is a file
print(path.parent.is_dir())  # Check if parent path is a directory

print(path.name)      # Filename: example.txt
print(path.stem)      # Filename without extension: example
print(path.suffix)    # Extension: .txt
print(path.parent)    # Parent directory
```

## Advanced Concepts

### Generators and Iterators
```python
# Iterator: an object with __iter__ and __next__ methods
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

# Using an iterator
for i in CountDown(5):
    print(i)  # Outputs: 5, 4, 3, 2, 1

# Generator function (uses yield)
def countdown(start):
    while start > 0:
        yield start
        start -= 1

# Using a generator
for i in countdown(5):
    print(i)  # Outputs: 5, 4, 3, 2, 1

# Generator expressions (similar to list comprehensions)
squares = (x**2 for x in range(5))
for square in squares:
    print(square)  # Outputs: 0, 1, 4, 9, 16
```

### Decorators
```python
# Basic decorator
def my_decorator(func):
    def wrapper():
        print("Something before the function is called.")
        func()
        print("Something after the function is called.")
    return wrapper

# Using the decorator
@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Something before the function is called.
# Hello!
# Something after the function is called.

# Decorator with arguments
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(n):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")
    return name

greet("Alice")
# Output:
# Hello, Alice!
# Hello, Alice!
# Hello, Alice!
```

### Context Managers
```python
# Using with statement with built-in context managers
with open("example.txt", "w") as file:
    file.write("Hello, World!")
# File is automatically closed after the block

# Creating a context manager using a class
class MyContextManager:
    def __init__(self, name):
        self.name = name
    
    def __enter__(self):
        print(f"Entering {self.name}")
        return self  # The value assigned to 'as' variable
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"Exiting {self.name}")
        # Return True to suppress exceptions
        if exc_type:
            print(f"Exception occurred: {exc_val}")
            return True

# Using our context manager
with MyContextManager("example") as cm:
    print("Inside the context")
    # raise ValueError("An error")  # Would be suppressed

# Creating a context manager using contextlib
from contextlib import contextmanager

@contextmanager
def my_context(name):
    print(f"Entering {name}")
    try:
        yield name  # The value assigned to 'as' variable
    finally:
        print(f"Exiting {name}")

# Using our contextlib-based context manager
with my_context("example") as name:
    print(f"Inside {name} context")
```

### Functional Programming
```python
# Map: apply function to each item in iterable
numbers = [1, 2, 3, 4, 5]
squared = map(lambda x: x**2, numbers)
print(list(squared))  # Output: [1, 4, 9, 16, 25]

# Filter: filter items based on a function
even = filter(lambda x: x % 2 == 0, numbers)
print(list(even))  # Output: [2, 4]

# Reduce: apply function cumulatively to items
from functools import reduce
product = reduce(lambda x, y: x * y, numbers)
print(product)  # Output: 120 (1*2*3*4*5)

# Partial functions: fix some arguments of a function
from functools import partial
def power(base, exponent):
    return base ** exponent

square = partial(power, exponent=2)
cube = partial(power, exponent=3)

print(square(4))  # Output: 16
print(cube(4))    # Output: 64
```

### Type Hints (Python 3.5+)
```python
# Basic type hints
def greet(name: str) -> str:
    return f"Hello, {name}!"

# Multiple argument types
def process_item(item_id: int, item_name: str) -> bool:
    print(f"Processing {item_name} with ID {item_id}")
    return True

# Type hints for collections
from typing import List, Dict, Tuple, Set, Optional, Union

def process_numbers(numbers: List[int]) -> int:
    return sum(numbers)

def process_student(student: Dict[str, Union[str, int]]) -> None:
    print(f"Name: {student['name']}, Age: {student['age']}")

def get_coordinates() -> Tuple[float, float]:
    return 10.5, 20.5

def may_return_none(value: bool) -> Optional[str]:
    if value:
        return "Value is True"
    return None
```

### Asynchronous Programming
```python
import asyncio

# Basic async function
async def say_hello():
    print("Hello")
    await asyncio.sleep(1)  # Non-blocking sleep
    print("World")

# Running async function
async def main():
    await say_hello()

# For Python 3.7+
# asyncio.run(main())

# For older versions
# loop = asyncio.get_event_loop()
# loop.run_until_complete(main())

# Multiple coroutines
async def count_up(name, start):
    print(f"{name} starts at {start}")
    for i in range(start, start + 3):
        print(f"{name}: {i}")
        await asyncio.sleep(0.5)
    print(f"{name} finished")
    return name

async def main_multiple():
    # Run coroutines concurrently
    results = await asyncio.gather(
        count_up("Counter 1", 1),
        count_up("Counter 2", 10),
        count_up("Counter 3", 100)
    )
    print(f"Results: {results}")

# asyncio.run(main_multiple())
```

## Python Development Tools

### Virtual Environments
```bash
# Creating a virtual environment
python -m venv myenv

# Activating the environment
# On Windows:
myenv\Scripts\activate
# On Unix or MacOS:
source myenv/bin/activate

# Installing packages in the environment
pip install package-name

# Deactivating the environment
deactivate

# Creating requirements.txt
pip freeze > requirements.txt

# Installing from requirements.txt
pip install -r requirements.txt
```

### Package Management with pip
```bash
# Install a package
pip install package-name

# Install a specific version
pip install package-name==1.2.3

# Upgrade a package
pip install --upgrade package-name

# Uninstall a package
pip uninstall package-name

# List installed packages
pip list

# Show package information
pip show package-name

# Search for packages
pip search query
```

### Testing

#### Unit Testing with unittest
```python
import unittest

# Function to test
def add(a, b):
    return a + b

# Test class
class TestAddFunction(unittest.TestCase):
    def test_add_positive_numbers(self):
        self.assertEqual(add(1, 2), 3)
    
    def test_add_negative_numbers(self):
        self.assertEqual(add(-1, -2), -3)
    
    def test_add_mixed_numbers(self):
        self.assertEqual(add(-1, 2), 1)

# Run the tests
if __name__ == "__main__":
    unittest.main()
```

#### Testing with pytest
```python
# File: test_functions.py
def add(a, b):
    return a + b

# Test function
def test_add_positive():
    assert add(1, 2) == 3

def test_add_negative():
    assert add(-1, -2) == -3

def test_add_mixed():
    assert add(-1, 2) == 1

# Run with: pytest test_functions.py
```

### Debugging
```python
# Using print statements
def buggy_function(x):
    print(f"x = {x}")
    y = x + 5
    print(f"y = {y}")
    z = y * 2
    print(f"z = {z}")
    return z

# Using assert statements
def calculate_average(numbers):
    assert len(numbers) > 0, "Cannot calculate average of empty list"
    total = sum(numbers)
    return total / len(numbers)

# Using the debugger
import pdb

def complex_function(x):
    y = x * 2
    pdb.set_trace()  # Start the debugger here
    z = y + 5
    return z

# Using logging
import logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

def process_data(data):
    logging.info(f"Processing data: {data}")
    try:
        result = data * 2
        logging.info(f"Result: {result}")
        return result
    except Exception as e:
        logging.error(f"Error: {e}")
        return None
```

## Best Practices

### Code Style and Conventions
```python
# Follow PEP 8 standards
# Example function with proper style
def calculate_total(items, tax_rate=0.1):
    """
    Calculate the total price including tax.
    
    Args:
        items (list): List of prices.
        tax_rate (float, optional): The tax rate as a decimal. Defaults to 0.1.
    
    Returns:
        float: The total price including tax.
    """
    subtotal = sum(items)
    tax = subtotal * tax_rate
    return subtotal + tax

# Naming conventions
MAX_SIZE = 100       # Constants in UPPERCASE
class Person:        # Classes in CamelCase
    def run_fast(self):  # Methods in snake_case
        pass

variable_name = 42   # Variables in snake_case

# Line length (79-80 characters)
long_string = (
    "This is a very long string that would exceed the "
    "recommended line length, so we split it."
)
```

### Error Handling Best Practices
```python
# Be specific about exceptions
try:
    value = int(input("Enter a number: "))
    result = 10 / value
except ValueError:
    print("That's not a valid number!")
except ZeroDivisionError:
    print("Cannot divide by zero!")
    
# Don't catch exceptions you can't handle
try:
    with open("config.json", "r") as f:
        config = json.load(f)
except FileNotFoundError:
    print("Configuration file not found. Using defaults.")
    config = {"default": True}
except json.JSONDecodeError:
    print("Configuration file is invalid. Using defaults.")
    config = {"default": True}

# Clean up resources with finally or context managers
try:
    file = open("example.txt", "w")
    file.write("Hello, World!")
finally:
    file.close()  # Always close the file

# Or better, use context manager
with open("example.txt", "w") as file:
    file.write("Hello, World!")  # File is automatically closed
```

### Documentation
```python
def calculate_area(length, width):
    """
    Calculate the area of a rectangle.
    
    Args:
        length (float): The length of the rectangle.
        width (float): The width of the rectangle.
    
    Returns:
        float: The area of the rectangle.
    
    Raises:
        ValueError: If length or width is negative.
    
    Examples:
        >>> calculate_area(4, 5)
        20.0
        >>> calculate_area(2.5, 3)
        7.5
    """
    if length < 0 or width < 0:
        raise ValueError("Length and width must be positive")
    return length * width
```

### Project Structure
```
my_project/
│
├── README.md           # Project documentation
├── LICENSE             # License information
├── setup.py            # Package setup file
├── requirements.txt    # Dependencies
│
├── my_package/         # Main package
│   ├── __init__.py     # Package initialization
│   ├── module1.py      # Module 1
│   ├── module2.py      # Module 2
│   └── subpackage/     # Sub-package
│       ├── __init__.py
│       └── module3.py
│
├── tests/              # Test files
│   ├── test_module1.py
│   └── test_module2.py
│
└── docs/               # Documentation
    ├── index.md
    └── api.md
```

### Performance Tips
```python
# Use list comprehensions instead of for loops
squares1 = []
for i in range(1000):
    squares1.append(i**2)

# More efficient list comprehension
squares2 = [i**2 for i in range(1000)]

# Use generators for large sequences
large_sum = sum(x**2 for x in range(1000000))  # Memory efficient

# Use proper data structures
# Sets for membership testing
numbers = set([1, 2, 3, 4, 5])
if 3 in numbers:  # O(1) operation for sets
    print("Found!")

# Use built-in functions and libraries
import statistics
data = [1, 2, 3, 4, 5]
mean = statistics.mean(data)  # Faster than manual calculation

# String concatenation
parts = ["a", "b", "c", "d"]
# Slow for many iterations:
result = ""
for part in parts:
    result += part

# Better:
result = "".join(parts)
```

### Security Considerations
```python
# Avoid using eval() with untrusted input
user_input = "2 + 2"
# Dangerous:
# result = eval(user_input)  # Could execute harmful code

# Use parameterized SQL queries to prevent SQL injection
import sqlite3
conn = sqlite3.connect("database.db")
cursor = conn.cursor()

user_id = "user_input"
# Dangerous:
# cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")  # SQL Injection risk!

# Safe:
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))

# Use secrets module for security-sensitive operations
import secrets
token = secrets.token_hex(16)  # Generate a secure token

# Never hardcode sensitive information
# Dangerous:
# api_key = "my_secret_api_key"  # Don't do this!

# Better:
import os
api_key = os.environ.get("API_KEY")  # Get from environment variable
```

