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

# Files and Exceptions

```{contents}
:local:
:depth: 2
```

Now that you’ve mastered the basic skills you need to write organized programs that are easy to use, it’s time to think about making your programs even more relevant and usable. In this chapter you’ll learn to work with files so your programs can quickly analyze lots of data. You’ll learn to handle errors so your programs don’t crash when they encounter unexpected situations. You’ll learn about exceptions, which are special objects Python creates to manage errors that arise while a program is running. You’ll also learn about the json module, which allows you to save user data so it isn’t lost when your program stops running.

Learning to work with files and save data will make your programs easier for people to use. Users will be able to choose what data to enter and when to enter it. People can run your program, do some work, and then close the program and pick up where they left off later. Learning to handle exceptions will help you deal with situations in which files don’t exist and deal with other problems that can cause your programs to crash. This will make your programs more robust when they encounter bad data, whether it comes from innocent mistakes or from malicious attempts to break your programs. With the skills you’ll learn in this chapter, you’ll make your programs more applicable, usable, and stable.

- [READING FROM A FILE](#read)
- [WRITING TO A FILE](#write)
- [EXCEPTIONS](#exceptions)
- [STORING DATA](#store)
- [SUMMARY](#summary)

## READING FROM A FILE <a clsss='anchor' id='reading'></a>

An incredible amount of data is available in text files. Text files can contain weather data, traffic data, socioeconomic data, literary works, and more. Reading from a file is particularly useful in data analysis applications, but it’s also applicable to any situation in which you want to analyze or modify information stored in a file. For example, you can write a program that reads in the contents of a text file and rewrites the file with formatting that allows a browser to display it.

When you want to work with the information in a text file, the first step is to read the file into memory. You can read the entire contents of a file, or you can work through the file one line at a time.

### Reading an Entire File

To begin, we need a file with a few lines of text in it. Let’s start with a file that contains pi to 30 decimal places, with 10 decimal places per line:

```
pi_digits.txt

3.1415926535
  8979323846
  2643383279
```

All the files used in this lecture can be found under the **data** folder.

Here’s a program that opens this file, reads it, and prints the contents of the file to the screen:

```{code-cell} ipython3
with open('data/pi_digits.txt') as file_object:
    contents = file_object.read()
print(contents)
```

The first line of this program has a lot going on. Let’s start by looking at the open() function. To do any work with a file, even just printing its contents, you first need to open the file to access it. The open() function needs one argument: the name of the file you want to open. Python looks for this file in the directory where the program that’s currently being executed is stored. In this example, file_reader.py is currently running, so Python looks for pi_digits.txt in the directory where file_reader.py is stored. The open() function returns an object representing the file. Here, open('pi_digits.txt') returns an object representing pi_digits.txt. Python assigns this object to file_object, which we’ll work with later in the program.

The keyword with closes the file once access to it is no longer needed. Notice how we call open() in this program but not close(). You could open and close the file by calling open() and close(), but if a bug in your program prevents the close() method from being executed, the file may never close. This may seem trivial, but improperly closed files can cause data to be lost or corrupted. And if you call close() too early in your program, you’ll find yourself trying to work with a closed file (a file you can’t access), which leads to more errors. It’s not always easy to know exactly when you should close a file, but with the structure shown here, Python will figure that out for you. All you have to do is open the file and work with it as desired, trusting that Python will close it automatically when the with block finishes execution.

Once we have a file object representing pi_digits.txt, we use the read() method in the second line of our program to read the entire contents of the file and store it as one long string in contents. When we print the value of contents, we get the entire text file back.

The only difference between this output and the original file is the extra blank line at the end of the output. The blank line appears because read() returns an empty string when it reaches the end of the file; this empty string shows up as a blank line. If you want to remove the extra blank line, you can use rstrip() in the call to print():

```{code-cell} ipython3
with open('data/pi_digits.txt') as file_object:
    contents = file_object.read()
    print(contents.rstrip())
```

### File Paths

When you pass a simple filename like pi_digits.txt to the open() function, Python looks in the directory where the file that’s currently being executed (that is, your .py program file) is stored.

Sometimes, depending on how you organize your work, the file you want to open won’t be in the same directory as your program file. For example, you might store your program files in a folder called python_work; inside python_work, you might have another folder called text_files to distinguish your program files from the text files they’re manipulating. Even though text_files is in python_work, just passing open() the name of a file in text_files won’t work, because Python will only look in python_work and stop there; it won’t go on and look in text_files. To get Python to open files from a directory other than the one where your program file is stored, you need to provide a file path, which tells Python to look in a specific location on your system.

Because text_files is inside python_work, you could use a relative file path to open a file from text_files. A relative file path tells Python to look for a given location relative to the directory where the currently running program file is stored. For example, you’d write:

```
with open('text_files/filename.txt') as file_object:
```

This line tells Python to look for the desired .txt file in the folder text_files and assumes that text_files is located inside python_work (which it is).

*Windows systems use a backslash (\) instead of a forward slash (/) when displaying file paths, but you can still *use forward slashes in your code.\*

You can also tell Python exactly where the file is on your computer regardless of where the program that’s being executed is stored. This is called an absolute file path. You use an absolute path if a relative path doesn’t work. For instance, if you’ve put text_files in some folder other than python_work—say, a folder called other_files—then just passing open() the path 'text_files/filename.txt' won’t work because Python will only look for that location inside python_work. You’ll need to write out a full path to clarify where you want Python to look.

Absolute paths are usually longer than relative paths, so it’s helpful to assign them to a variable and then pass that variable to open():

```
file_path = '/home/ehmatthes/other_files/text_files/filename.txt'
with open(file_path) as file_object:
```

Using absolute paths, you can read files from any location on your system. For now it’s easiest to store files in the same directory as your program files or in a folder such as text_files within the directory that stores your program files.

_If you try to use backslashes in a file path, you’ll get an error because the backslash is used to escape characters in strings. For example, in the path "C:\path\to\file.txt", the sequence \t is interpreted as a tab. If you need to use backslashes, you can escape each one in the path, like this: "C:\\\path\\\to\\\file.txt"._

### Reading Line by Line

When you’re reading a file, you’ll often want to examine each line of the file. You might be looking for certain information in the file, or you might want to modify the text in the file in some way. For example, you might want to read through a file of weather data and work with any line that includes the word sunny in the description of that day’s weather. In a news report, you might look for any line with the tag `<headline>` and rewrite that line with a specific kind of formatting.

You can use a for loop on the file object to examine each line from a file one at a time:

```{code-cell} ipython3
filename = 'data/pi_digits.txt'

with open(filename) as file_object:
    for line in file_object:
        print(line)
```

```
➊ filename = 'pi_digits.txt'

➋ with open(filename) as file_object:
➌     for line in file_object:
          print(line)
```

At ➊ we assign the name of the file we’re reading from to the variable filename. This is a common convention when working with files. Because the variable filename doesn’t represent the actual file—it’s just a string telling Python where to find the file—you can easily swap out 'pi_digits.txt' for the name of another file you want to work with. After we call open(), an object representing the file and its contents is assigned to the variable file_object ➋. We again use the with syntax to let Python open and close the file properly. To examine the file’s contents, we work through each line in the file by looping over the file object ➌.

When we print each line, we find even more blank lines. These blank lines appear because an invisible newline character is at the end of each line in the text file. The print function adds its own newline each time we call it, so we end up with two newline characters at the end of each line: one from the file and one from print(). Using rstrip() on each line in the print() call eliminates these extra blank lines:

```{code-cell} ipython3
filename = 'data/pi_digits.txt'

with open(filename) as file_object:
    for line in file_object:
        print(line.rstrip())
```

### Making a List of Lines from a File

When you use with, the file object returned by open() is only available inside the with block that contains it. If you want to retain access to a file’s contents outside the with block, you can store the file’s lines in a list inside the block and then work with that list. You can process parts of the file immediately and postpone some processing for later in the program.

The following example stores the lines of pi_digits.txt in a list inside the with block and then prints the lines outside the with block:

```{code-cell} ipython3
filename = 'data/pi_digits.txt'

with open(filename) as file_object:
    lines = file_object.readlines()

for line in lines:
    print(line.rstrip())
```

```
filename = 'data/pi_digits.txt'

   with open(filename) as file_object:
➊     lines = file_object.readlines()

➋ for line in lines:
       print(line.rstrip())
```

At ➊ the readlines() method takes each line from the file and stores it in a list. This list is then assigned to lines, which we can continue to work with after the with block ends. At ➋ we use a simple for loop to print each line from lines. Because each item in lines corresponds to each line in the file, the output matches the contents of the file exactly.

### Working with a File’s Contents

After you’ve read a file into memory, you can do whatever you want with that data, so let’s briefly explore the digits of pi. First, we’ll attempt to build a single string containing all the digits in the file with no whitespace in it:

```{code-cell} ipython3
filename = 'data/pi_digits.txt'

with open(filename) as file_object:
    lines = file_object.readlines()

pi_string = ''
for line in lines:
    pi_string += line.rstrip()

print(pi_string)
print(len(pi_string))
```

The variable pi_string contains the whitespace that was on the left side of the digits in each line, but we can get rid of that by using strip() instead of rstrip():

```{code-cell} ipython3
filename = 'data/pi_digits.txt'

with open(filename) as file_object:
    lines = file_object.readlines()

pi_string = ''
for line in lines:
    pi_string += line.strip()

print(pi_string)
print(len(pi_string))
```

_Note: When Python reads from a text file, it interprets all text in the file as a string. If you read in a number and want to work with that value in a numerical context, you’ll have to convert it to an integer using the int() function or convert it to a float using the float() function._

### Large Files: One Million Digits

So far we’ve focused on analyzing a text file that contains only three lines, but the code in these examples would work just as well on much larger files. If we start with a text file that contains pi to 1,000,000 decimal places instead of just 30, we can create a single string containing all these digits. We don’t need to change our program at all except to pass it a different file. We’ll also print just the first 50 decimal places, so we don’t have to watch a million digits scroll by in the terminal:

```{code-cell} ipython3
filename = 'data/pi_million_digits.txt'

with open(filename) as file_object:
    lines = file_object.readlines()

pi_string = ''
for line in lines:
    pi_string += line.strip()

print(f"{pi_string[:52]}...")
print(len(pi_string))
```

Python has no inherent limit to how much data you can work with; you can work with as much data as your system’s memory can handle.

### Is Your Birthday Contained in Pi?

I’ve always been curious to know if my birthday appears anywhere in the digits of pi. Let’s use the program we just wrote to find out if someone’s birthday appears anywhere in the first million digits of pi. We can do this by expressing each birthday as a string of digits and seeing if that string appears anywhere in pi_string:

```{code-cell} ipython3
filename = 'data/pi_million_digits.txt'

with open(filename) as file_object:
    lines = file_object.readlines()

pi_string = ''
for line in lines:
    pi_string += line.strip()

# birthday = input("Enter your birthday, in the form mmddyy: ")
# if birthday in pi_string:
#     print("Your birthday appears in the first million digits of pi!")
# else:
#     print("Your birthday does not appear in the first million digits of pi.")
```

## WRITING TO A FILE <a clsss='anchor' id='writing'></a>

One of the simplest ways to save data is to write it to a file. When you write text to a file, the output will still be available after you close the terminal containing your program’s output. You can examine output after a program finishes running, and you can share the output files with others as well. You can also write programs that read the text back into memory and work with it again later.

### Writing to an Empty File

To write text to a file, you need to call open() with a second argument telling Python that you want to write to the file. To see how this works, let’s write a simple message and store it in a file instead of printing it to the screen:

```{code-cell} ipython3
filename = 'data/programming.txt'

with open(filename, 'w') as file_object:
    file_object.write("I love programming.")
```

```
➊ with open(filename, 'w') as file_object:
➋     file_object.write("I love programming.")
```

The call to open() in this example has two arguments ➊. The first argument is still the name of the file we want to open. The second argument, 'w', tells Python that we want to open the file in write mode. You can open a file in read mode ('r'), write mode ('w'), append mode ('a'), or a mode that allows you to read and write to the file ('r+'). If you omit the mode argument, Python opens the file in read-only mode by default.

The open() function automatically creates the file you’re writing to if it doesn’t already exist. However, be careful opening a file in write mode ('w') because if the file does exist, Python will erase the contents of the file before returning the file object.

At ➋ we use the write() method on the file object to write a string to the file. This program has no terminal output, but if you open the file programming.txt, you’ll see one line:

_NOTE: Python can only write strings to a text file. If you want to store numerical data in a text file, you’ll have to convert the data to string format first using the str() function._

### Writing Multiple Lines

The write() function doesn’t add any newlines to the text you write. So if you write more than one line without including newline characters, your file may not look the way you want it to:

```{code-cell} ipython3
filename = 'data/programming.txt'

with open(filename, 'w') as file_object:
    file_object.write("I love programming.")
    file_object.write("I love creating new games.")
```

If you open programming.txt, you’ll see the two lines squished together:

```
I love programming.I love creating new games.
```

Including newlines in your calls to write() makes each string appear on its own line:

```{code-cell} ipython3
filename = 'data/programming.txt'

with open(filename, 'w') as file_object:
    file_object.write("I love programming.\n")
    file_object.write("I love creating new games.\n")
```

The output now appears on separate lines:

```
I love programming.
I love creating new games.
```

You can also use spaces, tab characters, and blank lines to format your output, just as you’ve been doing with terminal-based output.

### Appending to a File

If you want to add content to a file instead of writing over existing content, you can open the file in append mode. When you open a file in append mode, Python doesn’t erase the contents of the file before returning the file object. Any lines you write to the file will be added at the end of the file. If the file doesn’t exist yet, Python will create an empty file for you.

Let’s modify the program by adding some new reasons we love programming to the existing file programming.txt:

```{code-cell} ipython3
filename = 'data/programming.txt'

with open(filename, 'a') as file_object:
    file_object.write("I also love finding meaning in large datasets.\n")
    file_object.write("I love creating apps that can run in a browser.\n")
```

## EXCEPTIONS <a clsss='anchor' id='exceptions'></a>

Python uses special objects called exceptions to manage errors that arise during a program’s execution. Whenever an error occurs that makes Python unsure what to do next, it creates an exception object. If you write code that handles the exception, the program will continue running. If you don’t handle the exception, the program will halt and show a traceback, which includes a report of the exception that was raised.

Exceptions are handled with try-except blocks. A try-except block asks Python to do something, but it also tells Python what to do if an exception is raised. When you use try-except blocks, your programs will continue running even if things start to go wrong. Instead of tracebacks, which can be confusing for users to read, users will see friendly error messages that you write.

### Handling the ZeroDivisionError Exception

Let’s look at a simple error that causes Python to raise an exception. You probably know that it’s impossible to divide a number by zero, but let’s ask Python to do it anyway:

```{code-cell} ipython3
#print(5/0)
```

The error reported at ➊ in the traceback, ZeroDivisionError, is an exception object. Python creates this kind of object in response to a situation where it can’t do what we ask it to. When this happens, Python stops the program and tells us the kind of exception that was raised. We can use this information to modify our program. We’ll tell Python what to do when this kind of exception occurs; that way, if it happens again, we’re prepared.

### Using try-except Blocks

When you think an error may occur, you can write a try-except block to handle the exception that might be raised. You tell Python to try running some code, and you tell it what to do if the code results in a particular kind of exception.

Here’s what a try-except block for handling the ZeroDivisionError exception looks like:

```{code-cell} ipython3
try:
    print(5/0)
except ZeroDivisionError:
    print("You can't divide by zero!")
```

We put print(5/0), the line that caused the error, inside a try block. If the code in a try block works, Python skips over the except block. If the code in the try block causes an error, Python looks for an except block whose error matches the one that was raised and runs the code in that block.

In this example, the code in the try block produces a ZeroDivisionError, so Python looks for an except block telling it how to respond. Python then runs the code in that block, and the user sees a friendly error message instead of a traceback:

```
You can't divide by zero!
```

If more code followed the try-except block, the program would continue running because we told Python how to handle the error. Let’s look at an example where catching an error can allow a program to continue running.

### Using Exceptions to Prevent Crashes

Handling errors correctly is especially important when the program has more work to do after the error occurs. This happens often in programs that prompt users for input. If the program responds to invalid input appropriately, it can prompt for more valid input instead of crashing.

Let’s create a simple calculator that does only division:

```{code-cell} ipython3
print("Give me two numbers, and I'll divide them.")
print("Enter 'q' to quit.")

# while True:
#     first_number = input("\nFirst number: ")
#     if first_number == 'q':
#         break
#     second_number = input("Second number: ")
#     if second_number == 'q':
#         break
#     try:
#         answer = int(first_number) / int(second_number)
#     except ZeroDivisionError:
#         print("You can't divide by 0!")
#     else:
#         print(answer)
```

The try-except-else block works like this: Python attempts to run the code in the try block. The only code that should go in a try block is code that might cause an exception to be raised. Sometimes you’ll have additional code that should run only if the try block was successful; this code goes in the else block. The except block tells Python what to do in case a certain exception arises when it tries to run the code in the try block. By anticipating likely sources of errors, you can write robust programs that continue to run even when they encounter invalid data and missing resources. Your code will be resistant to innocent user mistakes and malicious attacks.

### Handling the FileNotFoundError Exception

One common issue when working with files is handling missing files. The file you’re looking for might be in a different location, the filename may be misspelled, or the file may not exist at all. You can handle all of these situations in a straightforward way with a try-except block.

Let’s try to read a file that doesn’t exist. The following program tries to read in the contents of Alice in Wonderland, but I haven’t saved the file alice.txt in the same directory as alice.py:

```{code-cell} ipython3
# filename = 'alice.txt'

# with open(filename, encoding='utf-8') as f:
#     contents = f.read()
```

There are two changes here. One is the use of the variable f to represent the file object, which is a common convention. The second is the use of the encoding argument. This argument is needed when your system’s default encoding doesn’t match the encoding of the file that’s being read.

Python can’t read from a missing file, so it raises an exception.

The last line of the traceback reports a FileNotFoundError: this is the exception Python creates when it can’t find the file it’s trying to open. In this example, the open() function produces the error, so to handle it, the try block will begin with the line that contains open():

```{code-cell} ipython3
filename = 'alice.txt'

try:
    with open(filename, encoding='utf-8') as f:
        contents = f.read()
except FileNotFoundError:
    print(f"Sorry, the file {filename} does not exist.")
```

### Analyzing Text

You can analyze text files containing entire books. Many classic works of literature are available as simple text files because they are in the public domain. The texts used in this section come from Project Gutenberg (http://gutenberg.org/). Project Gutenberg maintains a collection of literary works that are available in the public domain, and it’s a great resource if you’re interested in working with literary texts in your programming projects.

Let’s pull in the text of Alice in Wonderland and try to count the number of words in the text. We’ll use the string method split(), which can build a list of words from a string. Here’s what split() does with a string containing just the title "Alice in Wonderland":

```{code-cell} ipython3
title = "Alice in Wonderland"
title.split()
```

The split() method separates a string into parts wherever it finds a space and stores all the parts of the string in a list. The result is a list of words from the string, although some punctuation may also appear with some of the words. To count the number of words in Alice in Wonderland, we’ll use split() on the entire text. Then we’ll count the items in the list to get a rough idea of the number of words in the text:

```{code-cell} ipython3
filename = 'data/alice.txt'

try:
    with open(filename, encoding='utf-8') as f:
        contents = f.read()
except FileNotFoundError:
    print(f"Sorry, the file {filename} does not exist.")
else:
    # Count the approximate number of words in the file.
    words = contents.split()
    num_words = len(words)
    print(f"The file {filename} has about {num_words} words.")
```

### Working with Multiple Files

Let’s add more books to analyze. But before we do, let’s move the bulk of this program to a function called count_words(). By doing so, it will be easier to run the analysis for multiple books:

```{code-cell} ipython3
def count_words(filename):
    """Count the approximate number of words in a file."""
    try:
        with open(filename, encoding='utf-8') as f:
            contents = f.read()
    except FileNotFoundError:
        pass
    else:
        words = contents.split()
        num_words = len(words)
        print(f"The file {filename} has about {num_words} words.")

filename = 'data/alice.txt'
count_words(filename)
```

Now we can write a simple loop to count the words in any text we want to analyze. We do this by storing the names of the files we want to analyze in a list, and then we call count_words() for each file in the list. We’ll try to count the words for Alice in Wonderland, Siddhartha, Moby Dick, and Little Women, which are all available in the public domain. I’ve intentionally left siddhartha.txt out of the directory containing word_count.py, so we can see how well our program handles a missing file:

```{code-cell} ipython3
def count_words(filename):
    """Count the approximate number of words in a file."""
    try:
        with open(filename, encoding='utf-8') as f:
            contents = f.read()
    except FileNotFoundError:
        print(f"Sorry, the file {filename} does not exist.")
    else:
        words = contents.split()
        num_words = len(words)
        print(f"The file {filename} has about {num_words} words.")

filenames = ['alice.txt', 'siddhartha.txt', 'moby_dick.txt', 'little_women.txt', 'hello_world.txt']
filenames = ["data/"+filename for filename in filenames]
for filename in filenames:
    count_words(filename)
```

Using the try-except block in this example provides two significant advantages. We prevent our users from seeing a traceback, and we let the program continue analyzing the texts it’s able to find. If we don’t catch the FileNotFoundError that siddhartha.txt raised, the user would see a full traceback, and the program would stop running after trying to analyze Siddhartha. It would never analyze Moby Dick or Little Women.

### Failing Silently

In the previous example, we informed our users that one of the files was unavailable. But you don’t need to report every exception you catch. Sometimes you’ll want the program to fail silently when an exception occurs and continue on as if nothing happened. To make a program fail silently, you write a try block as usual, but you explicitly tell Python to do nothing in the except block. Python has a pass statement that tells it to do nothing in a block:

```{code-cell} ipython3
def count_words(filename):
    """Count the approximate number of words in a file."""
    try:
        with open(filename, encoding='utf-8') as f:
            contents = f.read()
    except FileNotFoundError:
        pass
    else:
        words = contents.split()
        num_words = len(words)
        print(f"The file {filename} has about {num_words} words.")

filenames = ['alice.txt', 'siddhartha.txt', 'moby_dick.txt', 'little_women.txt', 'hello_world.txt']
filenames = ["data/"+filename for filename in filenames]
for filename in filenames:
    count_words(filename)
```

The pass statement also acts as a placeholder. It’s a reminder that you’re choosing to do nothing at a specific point in your program’s execution and that you might want to do something there later. For example, in this program we might decide to write any missing filenames to a file called missing_files.txt. Our users wouldn’t see this file, but we’d be able to read the file and deal with any missing texts.

### Deciding Which Errors to Report

How do you know when to report an error to your users and when to fail silently? If users know which texts are supposed to be analyzed, they might appreciate a message informing them why some texts were not analyzed. If users expect to see some results but don’t know which books are supposed to be analyzed, they might not need to know that some texts were unavailable. Giving users information they aren’t looking for can decrease the usability of your program. Python’s error-handling structures give you fine-grained control over how much to share with users when things go wrong; it’s up to you to decide how much information to share.

Well-written, properly tested code is not very prone to internal errors, such as syntax or logical errors. But every time your program depends on something external, such as user input, the existence of a file, or the availability of a network connection, there is a possibility of an exception being raised. A little experience will help you know where to include exception handling blocks in your program and how much to report to users about errors that arise.

## STORING DATA <a clsss='anchor' id='storing-data'></a>

Many of your programs will ask users to input certain kinds of information. You might allow users to store preferences in a game or provide data for a visualization. Whatever the focus of your program is, you’ll store the information users provide in data structures such as lists and dictionaries. When users close a program, you’ll almost always want to save the information they entered. A simple way to do this involves storing your data using the json module.

The json module allows you to dump simple Python data structures into a file and load the data from that file the next time the program runs. You can also use json to share data between different Python programs. Even better, the JSON data format is not specific to Python, so you can share data you store in the JSON format with people who work in many other programming languages. It’s a useful and portable format, and it’s easy to learn.

### Using json.dump() and json.load()

Let’s write a short program that stores a set of numbers and another program that reads these numbers back into memory. The first program will use json.dump() to store the set of numbers, and the second program will use json.load().

The json.dump() function takes two arguments: a piece of data to store and a file object it can use to store the data. Here’s how you can use json.dump() to store a list of numbers:

```{code-cell} ipython3
import json

numbers = [2, 3, 5, 7, 11, 13]

filename = 'data/numbers.json'
with open(filename, 'w') as f:
    json.dump(numbers, f)
```

```
import json

   numbers = [2, 3, 5, 7, 11, 13]

➊ filename = 'numbers.json'
➋ with open(filename, 'w') as f:
➌     json.dump(numbers, f)
```

We first import the json module and then create a list of numbers to work with. At ➊ we choose a filename in which to store the list of numbers. It’s customary to use the file extension .json to indicate that the data in the file is stored in the JSON format. Then we open the file in write mode, which allows json to write the data to the file ➋. At ➌ we use the json.dump() function to store the list numbers in the file numbers.json.

This program has no output, but let’s open the file numbers.json and look at it. The data is stored in a format that looks just like Python.

```
[2, 3, 5, 7, 11, 13]
```

Now we’ll write a program that uses json.load() to read the list back into memory:

```{code-cell} ipython3
import json

filename = 'data/numbers.json'
with open(filename) as f:
    numbers = json.load(f)

print(numbers)
```

### Saving and Reading User-Generated Data

Saving data with json is useful when you’re working with user-generated data, because if you don’t store your user’s information somehow, you’ll lose it when the program stops running. Let’s look at an example where we prompt the user for their name the first time they run a program and then remember their name when they run the program again.

Let’s start by storing the user’s name:

```{code-cell} ipython3
import json

def get_stored_username():
    """Get stored username if available."""
    filename = 'data/username.json'
    try:
        with open(filename) as f:
            username = json.load(f)
    except FileNotFoundError:
        return None
    else:
        return username

def get_new_username():
    """Prompt for a new username."""
    username = input("What is your name? ")
    filename = 'data/username.json'
    with open(filename, 'w') as f:
        json.dump(username, f)
    return username

def greet_user():
    """Greet the user by name."""
    username = get_stored_username()
    if username:
        print(f"Welcome back, {username}!")
    else:
        username = get_new_username()
        print(f"We'll remember you when you come back, {username}!")

greet_user()
```

```{code-cell} ipython3
greet_user()
```

Each function in this final version of remember_me.py has a single, clear purpose. We call greet_user(), and that function prints an appropriate message: it either welcomes back an existing user or greets a new user. It does this by calling get_stored_username(), which is responsible only for retrieving a stored username if one exists. Finally, greet_user() calls get_new_username() if necessary, which is responsible only for getting a new username and storing it. This compartmentalization of work is an essential part of writing clear code that will be easy to maintain and extend.

## SUMMARY <a clsss='anchor' id='summary'></a>

In this chapter, you learned how to work with files. You learned to read an entire file at once and read through a file’s contents one line at a time. You learned to write to a file and append text onto the end of a file. You read about exceptions and how to handle the exceptions you’re likely to see in your programs. Finally, you learned how to store Python data structures so you can save information your users provide, preventing them from having to start over each time they run a program.
