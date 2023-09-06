---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Getting Started

```{contents}
:local:
:depth: 2
```

## Jupyter Notebook Keyboard Shortcuts

- **Shift-Enter**: run cell, select below
- **Ctrl-Enter**: : run selected cells
- **Alt-Enter**: run cell and insert below
- **Tab**: code completion or indent
- **Shift-Tab**: tooltip
- **Matplotlib plotting**: %matplotlib inline

## Variables and Data Types

The code demonstrates the declaration and assignment of variables with various data types, such as strings, integers, floats, and booleans.

```{code-cell} ipython3
# Variables
name = "John"
age = 25
salary = 2500.50
is_student = True
```

## Basic Operations

The code showcases basic arithmetic operations (addition, subtraction, multiplication, division, modulus) and comparison operations (equality, inequality, greater than, less than, greater than or equal to, less than or equal to), as well as logical operations (AND, OR, NOT).

```{code-cell} ipython3
# Arithmetic operations
result = 10 + 5
result = 10 - 5
result = 10 * 5
result = 10 / 5
result = 10 % 3
```

```{code-cell} ipython3
# Comparison operations
is_equal = 10 == 5
is_not_equal = 10 != 5
is_greater = 10 > 5
is_less = 10 < 5
is_greater_equal = 10 >= 5
is_less_equal = 10 <= 5
```

```{code-cell} ipython3
# Logical operations
is_true = True and False
is_true = True or False
is_true = not False
```

## Control Flow

The code demonstrates the use of if-elif-else statements to control the flow of the program based on different conditions. It also showcases for and while loops for iterating over a sequence of elements or executing a block of code repeatedly until a condition is met.

```{code-cell} ipython3
# If statement
x = 10
if x > 0:
    print("Positive number")
elif x < 0:
    print("Negative number")
else:
    print("Zero")
```

```{code-cell} ipython3
# For loop
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

```{code-cell} ipython3
# While loop
count = 0
while count < 5:
    print(count)
    count += 1
```

## Data Structures

The code showcases the creation and manipulation of essential data structures in Python, including lists (a collection of elements enclosed in square brackets), tuples (an immutable sequence of elements enclosed in parentheses), and dictionaries (a collection of key-value pairs enclosed in curly braces).

```{code-cell} ipython3
# Lists
numbers = [1, 2, 3, 4, 5]
names = ["Alice", "Bob", "Charlie"]
```

```{code-cell} ipython3
# Tuples
point = (10, 20)
dimensions = (100, 200, 300)
```

```{code-cell} ipython3
# Dictionaries
person = {"name": "John", "age": 25, "city": "New York"}
```

## Functions

The code demonstrates the definition and usage of a function. It defines a function named "say_hello" that prints "Hello, World!" when called.

```{code-cell} ipython3
# Function definition
def say_hello():
    print("Hello, World!")

# Function call
say_hello()
```

## Input and Output

The code demonstrates how to take user input using the input() function and display output using the print() function.

```{code-cell} ipython3
# Taking user input
name = input("Enter your name: ")
print("Hello,", name)

# Displaying output
print("Hello, World!")
```

## File Handling

The code showcases file handling operations. It demonstrates how to read the contents of a file using the open() function with the "r" mode and how to write to a file using the open() function with the "w" mode.

```{code-cell} ipython3
# Writing to a file
with open('example.txt', 'w') as file:
    file.write('Hello, world!')

# Reading from a file
with open('example.txt', 'r') as file:
    content = file.read()
    print("File content:", content)
```

## Exception Handling

The code demonstrates the use of try-except blocks to handle exceptions. It takes two numbers as input, performs division, and handles potential exceptions such as ValueError (when non-numeric input is provided) and ZeroDivisionError (when dividing by zero).

```{code-cell} ipython3
# Handling exceptions
try:
    num1 = int(input("Enter a number: "))
    num2 = int(input("Enter another number: "))
    result = num1 / num2
    print("Result:", result)
except ValueError:
    print("Invalid input. Please enter a number.")
except ZeroDivisionError:
    print("Cannot divide by zero.")
```
