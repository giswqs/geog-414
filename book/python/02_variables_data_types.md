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

# Variables and Data Types

```{contents}
:local:
:depth: 2
```

In this lecture you’ll learn about the different kinds of data you can work with in your Python programs. You’ll also learn how to use variables to represent data in your programs.

- [Variables](#variables)
- [Strings](#strings)
- [Numbers](#numbers)
- [Comments](#comments)

## Variables

```{code-cell} ipython3
message = "Hello Python World!"
print(message)
```

We’ve added a _variable_ named message. Every variable is connected to a value, which is the information associated with that variable. In this case the value is the `"Hello Python world!"` text. Adding a variable makes a little more work for the Python interpreter. When it processes the first line, it associates the variable message with the `"Hello Python world!"` text. When it reaches the second line, it prints the value associated with message to the screen. Let’s expand on this program by adding two new lines of code:

```{code-cell} ipython3
message = "Hello Python Crash Course World!"
print(message)
```

You can change the value of a variable in your program at any time, and Python will always keep track of its current value.

### Naming and Using Variables

When you’re using variables in Python, you need to adhere to a few rules and guidelines. Breaking some of these rules will cause errors; other guidelines just help you write code that’s easier to read and understand. Be sure to keep the following variable rules in mind:

- Variable names can contain only letters, numbers, and underscores. They can start with a letter or an underscore, but not with a number. For instance, you can call a variable `message_1` but not `1_message`.
- Spaces are not allowed in variable names, but underscores can be used to separate words in variable names. For example, `greeting_message` works, but `greeting message` will cause errors.
- Avoid using Python keywords and function names as variable names; that is, do not use words that Python has reserved for a particular pro- grammatic purpose, such as the word print. (See [Python Keywords and Built-in Functions](http://bit.ly/2UjAebR))
- Variable names should be short but descriptive. For example, `name` is better than `n`, `student_name` is better than `s_n`, and `name_length` is better than `length_of_persons_name`.
- Be careful when using the lowercase letter `l` and the uppercase letter `O` because they could be confused with the numbers `1` and `0`.

**Note**: The Python variables you’re using at this point should be lowercase. You won’t get errors if you use uppercase letters, but uppercase letters in variable names have special meanings that we’ll discuss in later chapters.

### Avoiding Name Errors When Using Variables

Every programmer makes mistakes, and most make mistakes every day. Although good programmers might create errors, they also know how to respond to those errors efficiently. Let’s look at an error you’re likely to make early on and learn how to fix it. We’ll write some code that generates an error on purpose. Enter the following code, including the misspelled word `message` shown in bold:

```{code-cell} ipython3
message = "Hello Python Crash Course reader!"
# print(message)
```

When an error occurs in your program, the Python interpreter does its best to help you figure out where the problem is. The interpreter provides a traceback when a program cannot run successfully. A traceback is a record of where the interpreter ran into trouble when trying to execute your code. Here’s an example of the traceback that Python provides after you’ve accidentally misspelled a variable’s name. Let's fix it:

```{code-cell} ipython3
message = "Hello Python Crash Course reader!"
print(message)
```

### Strings

A _string_ is a series of characters. Anything inside quotes is considered a string in Python, and you can use single or double quotes around your strings like this:

```
"This is a string."
'This is also a string.'
```

This flexibility allows you to use quotes and apostrophes within your strings:

```
'I told my friend, "Python is my favorite language!"'
"The language 'Python' is named after Monty Python, not the snake."
"One of Python's strengths is its diverse and supportive community."
```

### Changing Case in a String with Methods

One of the simplest tasks you can do with strings is change the case of the words in a string. Look at the following code, and try to determine what’s happening:

```{code-cell} ipython3
name = "ada lovelace"
print(name.title())
```

In this example, the variable name refers to the lowercase string "ada lovelace". The method title() appears after the variable in the print() call. A method is an action that Python can perform on a piece of data. The dot (.) after name in name.title() tells Python to make the title() method act on the variable name. Every method is followed by a set of parentheses, because methods often need additional information to do their work. That informa- tion is provided inside the parentheses. The title() function doesn’t need any additional information, so its parentheses are empty. The title() method changes each word to title case, where each word begins with a capital letter. This is useful because you’ll often want to think of a name as a piece of information. For example, you might want your pro- gram to recognize the input values Ada, ADA, and ada as the same name, and display all of them as Ada. Several other useful methods are available for dealing with case as well. For example, you can change a string to all uppercase or all lowercase letters like this:

```{code-cell} ipython3
name = "Ada Lovelace"
print(name.upper())
print(name.lower())
```

### Using Variables in Strings

In some situations, you’ll want to use a variable’s value inside a string. For example, you might want two variables to represent a first name and a last name respectively, and then want to combine those values to display someone’s full name:

```{code-cell} ipython3
first_name = "ada"
last_name = "lovelace"
full_name = f"{first_name} {last_name}"
print(full_name)
```

To insert a variable’s value into a string, place the letter f immediately before the opening quotation mark. Put braces around the name or names of any variable you want to use inside the string. Python will replace each variable with its value when the string is displayed. These strings are called f-strings. The f is for format, because Python formats the string by replacing the name of any variable in braces with its value.

```{code-cell} ipython3
first_name = "ada"
last_name = "lovelace"
full_name = f"{first_name} {last_name}"
message = f"Hello, {full_name.title()}!"
print(message)
```

```{code-cell} ipython3
full_name = "{} {}".format(first_name, last_name)
print(full_name)
```

### Adding Whitespace to Strings with Tabs or Newlines

In programming, whitespace refers to any nonprinting character, such as spaces, tabs, and end-of-line symbols. You can use whitespace to organize your output so it’s easier for users to read.
To add a tab to your text, use the character combination \t as shown at:

```{code-cell} ipython3
print("Python")
print("\tPython")
```

To add a newline in a string, use the character combination \n:

```{code-cell} ipython3
print("Languages:\nPython\nC\nJavaScript")
```

You can also combine tabs and newlines in a single string. The string "\n\t" tells Python to move to a new line, and start the next line with a tab. The following example shows how you can use a one-line string to generate
four lines of output:

```{code-cell} ipython3
print("Languages:\n\tPython\n\tC\n\tJavaScript")
```

### Stripping Whitespace

Extra whitespace can be confusing in your programs. To programmers 'python' and 'python ' look pretty much the same. But to a program, they are two different strings. Python detects the extra space in 'python ' and
considers it significant unless you tell it otherwise.
whitespace from data that people enter. Python can look for extra whitespace on the right and left sides of a
string. To ensure that no whitespace exists at the right end of a string, use the rstrip() method.

```{code-cell} ipython3
favorite_language = 'python '
print(favorite_language)
print(favorite_language.rstrip())
```

```{code-cell} ipython3
favorite_language = ' python '
print(favorite_language.rstrip())
print(favorite_language.lstrip())
print(favorite_language.strip())
```

### Avoiding Syntax Errors with Strings

One kind of error that you might see with some regularity is a syntax error. A syntax error occurs when Python doesn’t recognize a section of your pro gram as valid Python code. For example, if you use an apostrophe within
single quotes, you’ll produce an error. This happens because Python interprets everything between the first single quote and the apostrophe as a string. It then tries to interpret the rest of the text as Python code, which
causes errors.

```{code-cell} ipython3
message = "One of Python's strengths is its diverse community."
print(message)
```

```{code-cell} ipython3
# message = 'One of Python's strengths is its diverse community.'
# print(message)
```

## Numbers

Numbers are used quite often in programming to keep score in games, represent data in visualizations, store information in web applications, and so on. Python treats numbers in several different ways, depending on how they’re being used. Let’s first look at how Python manages integers, because they’re the simplest to work with.

### Integers

You can add ( +), subtract ( -), multiply (\*), and divide ( /) integers in Python.

```{code-cell} ipython3
2 + 3
```

```{code-cell} ipython3
3 - 2
```

```{code-cell} ipython3
2 * 3
```

```{code-cell} ipython3
3 / 2
```

Python simply returns the result of the Python uses two multiplication symbols to represent exponents:

```{code-cell} ipython3
3 ** 2
```

```{code-cell} ipython3
3 ** 3
```

```{code-cell} ipython3
10 ** 6
```

Python supports the order of operations too, so you can use multiple operations in one expression. You can also use parentheses to modify the order of operations so Python can evaluate your expression in the order you specify. For example:

```{code-cell} ipython3
2 + 3*4
```

```{code-cell} ipython3
(2 + 3) * 4
```

### Floats

Python calls any number with a decimal point a float. This term is used in most programming languages, and it refers to the fact that a decimal point can appear at any position in a number. Every programming language must be carefully designed to properly manage decimal numbers so numbers behave appropriately no matter where the decimal point appears.

```{code-cell} ipython3
0.1 + 0.1
```

```{code-cell} ipython3
2 * 0.1
```

But be aware that you can sometimes get an arbitrary number of decimal places in your answer. This happens in all languages and is of little concern. Python tries to find a way to represent the result as precisely as possible, which is sometimes difficult given how computers have to represent numbers internally. Just ignore the extra decimal places for now; you’ll learn ways to deal with this later.

```{code-cell} ipython3
0.2 + 0.1
```

### Integers and Floats

When you divide any two numbers, even if they are integers that result in a whole number, you’ll always get a float:

```{code-cell} ipython3
4 / 2
```

If you mix an integer and a float in any other operation, you’ll get a float as well:

```{code-cell} ipython3
1 + 2.0
```

### Underscores in Numbers

When you’re writing long numbers, you can group digits using underscores to make large numbers more readable. When you print a number that was defined using underscores, Python prints only the digits. Python ignores the underscores when storing these kinds of values. Even if you don’t group the digits in threes, the value will still be un­a ffected. To Python, 1000 is the same as 1_000, which is the same as 10_00. This feature works for integers and floats, but it’s only available in Python 3.6

```{code-cell} ipython3
universe_age = 14_000_000_000
print(universe_age)
```

### Multiple Assignment

You can assign values to more than one variable using just a single line. This can help shorten your programs and make them easier to read; you’ll use this technique most often when initializing a set of numbers. For example, here’s how you can initialize the variables x, y, and z to zero:

```{code-cell} ipython3
x, y, z = 0, 0, 0
```

You need to separate the variable names with commas, and do the same with the values, and Python will assign each value to its respectively positioned variable. As long as the number of values matches the number of variables, Python will match them up correctly.

### Constants

A constant is like a variable whose value stays the same throughout the life of a program. Python doesn’t have built-in constant types, but Python programmers use all capital letters to indicate a variable should be treated as a constant and never be changed:

```{code-cell} ipython3
MAX_CONNECTIONS = 5000
```

## Comments

Comments are an extremely useful feature in most programming languages. Everything you’ve written in your programs so far is Python code. As your programs become longer and more complicated, you should add notes within your programs that describe your overall approach to the problem you’re solving. A comment allows you to write notes in English within your programs.

### How Do You Write Comments?

In Python, the hash mark (#) indicates a comment. Anything following a hash mark in your code is ignored by the Python interpreter. For example:

```{code-cell} ipython3
# Say hello to everyone.
print("Hello Python people!")
```

### Shortcut for making comments in Jupyter Notebook

- Ctrl + /

### What Kind of Comments Should You Write?

The main reason to write comments is to explain what your code is supposed to do and how you are making it work. When you’re in the middle of working on a project, you understand how all of the pieces fit together. But when you return to a project after some time away, you’ll likely have forgotten some of the details. You can always study your code for a while and figure out how segments were supposed to work, but writing good comments can save you time by summarizing your overall approach in clear English. If you want to become a professional programmer or collaborate with other programmers, you should write meaningful comments. Today, most software is written collaboratively, whether by a group of employees at one company or a group of people working together on an open source project. Skilled programmers expect to see comments in code, so it’s best to start adding descriptive comments to your programs now. Writing clear, concise comments in your code is one of the most beneficial habits you can form as a new programmer.
