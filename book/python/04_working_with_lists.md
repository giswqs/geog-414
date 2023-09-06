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

# Working with Lists

```{contents}
:local:
:depth: 2
```

In this lecture you’ll learn how to loop through an entire list using just a few lines of code regardless of how long the list is. Looping allows you to take the same action, or set of actions, with every item in a list. As a result, you’ll be able to work efficiently with lists of any length, including those with thousands or even millions of items.

- [Looping Through an Entire List](#what-is-a-list)
- [Avoiding Indentation Errors](#changing-elements)
- [Making Numerical Lists](#organizing-a-list)
- [Working with Part of a List](#avoiding-index-errors)
- [Tuples](#organizing-a-list)
- [Styling Your Code](#avoiding-index-errors)

## Looping Through an Entire List

You’ll often want to run through all entries in a list, performing the same task with each item. For example, in a game you might want to move every element on the screen by the same amount, or in a list of numbers you might want to perform the same statistical operation on every element. Or perhaps you’ll want to display each headline from a list of articles on a website. When you want to do the same action with every item in a list, you can use Python’s for loop.
Let’s say we have a list of magicians’ names, and we want to print out each name in the list. We could do this by retrieving each name from the list individually, but this approach could cause several problems. For one, it would be repetitive to do this with a long list of names. Also, we’d have to change our code each time the list’s length changed. A for loop avoids both of these issues by letting Python manage these issues internally. Let’s use a for loop to print out each name in a list of magicians:

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(magician)
```

When you’re using loops for the first time, keep in mind that the set of steps is repeated once for each item in the list, no matter how many items are in the list. If you have a million items in your list, Python repeats these steps a million times—and usually very quickly. Also keep in mind when writing your own for loops that you can choose any name you want for the temporary variable that will be associated with each value in the list. However, it’s helpful to choose a meaningful name that represents a single item from the list.

These naming conventions can help you follow the action being done on each item within a for loop. Using singular and plural names can help you identify whether a section of code is working with a single element from the list or the entire list.

### Doing More Work Within a for Loop

You can do just about anything with each item in a for loop. Let’s build on the previous example by printing a message to each magician, telling them that they performed a great trick:

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
```

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
    print(f"I can't wait to see your next trick, {magician.title()}.\n")
```

### Doing Something After a for Loop

What happens once a for loop has finished executing? Usually, you’ll want to summarize a block of output or move on to other work that your program must accomplish. Any lines of code after the for loop that are not indented are executed once without repetition. Let’s write a thank you to the group of magicians as a whole, thanking them for putting on an excellent show. To display this group message after all of the individual messages have been printed, we place the thank you message after the for loop without indentation:

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
    print(f"I can't wait to see your next trick, {magician.title()}.\n")

print("Thank you, everyone. That was a great magic show!")
```

## Avoiding Indentation Errors

Python uses indentation to determine how a line, or group of lines, is related to the rest of the program. In the previous examples, the lines that printed messages to individual magicians were part of the for loop because they were indented. Python’s use of indentation makes code very easy to read. Basically, it uses whitespace to force you to write neatly formatted code with a clear visual structure. In longer Python programs, you’ll notice blocks of code indented at a few different levels. These indentation levels help you gain a general sense of the overall program’s organization.

As you begin to write code that relies on proper indentation, you’ll need to watch for a few common indentation errors. For example, people sometimes indent lines of code that don’t need to be indented or forget to indent lines that need to be indented. Seeing examples of these errors now will help you avoid them in the future and correct them when they do appear in your own programs.

Let’s examine some of the more common indentation errors.

### Forgetting to Indent

Always indent the line after the for statement in a loop. If you forget, Python will remind you:

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
# for magician in magicians:
# print(magician)
```

### Forgetting to Indent Additional Lines

Sometimes your loop will run without any errors but won’t produce the expected result. This can happen when you’re trying to do several tasks in a loop and you forget to indent some of its lines. For example, this is what happens when we forget to indent the second line in the loop that tells each magician we’re looking forward to their next trick:

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
print(f"I can't wait to see your next trick, {magician.title()}.\n")
```

### Indenting Unnecessarily

If you accidentally indent a line that doesn’t need to be indented, Python informs you about the unexpected indent:

```{code-cell} ipython3
# message = "Hello Python world!"
#     print(message)
```

### Indenting Unnecessarily

After the Loop If you accidentally indent code that should run after a loop has finished, that code will be repeated once for each item in the list. Sometimes this prompts Python to report an error, but often this will result in a logical error. For example, let’s see what happens when we accidentally indent the line that thanked the magicians as a group for putting on a good show:

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
    print(f"I can't wait to see your next trick, {magician.title()}.\n")

    print("Thank you everyone, that was a great magic show!")
```

How to print element index and content together?

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
for index, magician in enumerate(magicians):
    print(f"{index}: {magician}")
```

### Forgetting the Colon

The colon at the end of a for statement tells Python to interpret the next line as the start of a loop.

```{code-cell} ipython3
magicians = ['alice', 'david', 'carolina']
# for magician in magicians
#     print(magician)
```

## Making Numerical Lists

Many reasons exist to store a set of numbers. For example, you’ll need to keep track of the positions of each character in a game, and you might want to keep track of a player’s high scores as well. In data visualizations, you’ll almost always work with sets of numbers, such as temperatures, distances, population sizes, or latitude and longitude values, among other types of numerical sets.

Lists are ideal for storing sets of numbers, and Python provides a variety of tools to help you work efficiently with lists of numbers. Once you understand how to use these tools effectively, your code will work well even when your lists contain millions of items.

### Using the range() Function

Python’s range() function makes it easy to generate a series of numbers. For example, you can use the range() function to print a series of numbers like this:

```{code-cell} ipython3
for value in range(1, 5):
    print(value)
```

In this example, range() prints only the numbers 1 through 4. This is another result of the off-by-one behavior you’ll see often in programming languages. The range() function causes Python to start counting at the first value you give it, and it stops when it reaches the second value you provide. Because it stops at that second value, the output never contains the end value, which would have been 5 in this case.

```{code-cell} ipython3
for value in range(1, 6):
    print(value)
```

### Using range() to Make a List of Numbers

If you want to make a list of numbers, you can convert the results of range() directly into a list using the list() function. When you wrap list() around a call to the range() function, the output will be a list of numbers. In the example in the previous section, we simply printed out a series of numbers. We can use list() to convert that same set of numbers into a list:

```{code-cell} ipython3
numbers = list(range(1, 6))
print(numbers)
```

We can also use the range() function to tell Python to skip numbers in a given range. If you pass a third argument to range(), Python uses that value as a step size when generating numbers. For example, here’s how to list the even numbers between 1 and 10:

```{code-cell} ipython3
even_numbers = list(range(2, 11, 2))
print(even_numbers)
```

You can create almost any set of numbers you want to using the range() function. For example, consider how you might make a list of the first 10 square numbers (that is, the square of each integer from 1 through 10). In Python, two asterisks (\*\*) represent exponents. Here’s how you might put the first 10 square numbers into a list:

```{code-cell} ipython3
squares = []
for value in range(1, 11):
    square = value ** 2
    squares.append(square)
print(squares)
```

To write this code more concisely, omit the temporary variable square and append each new value directly to the list:

```{code-cell} ipython3
squares = []
for value in range(1,11):
    squares.append(value**2)
print(squares)
```

### Simple Statistics with a List of Numbers

A few Python functions are helpful when working with lists of numbers. For example, you can easily find the minimum, maximum, and sum of a list of numbers:

```{code-cell} ipython3
digits = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
min(digits)
```

```{code-cell} ipython3
max(digits)
```

```{code-cell} ipython3
sum(digits)
```

### List Comprehensions

The approach described earlier for generating the list squares consisted of using three or four lines of code. A list comprehension allows you to generate this same list in just one line of code. A list comprehension combines the for loop and the creation of new elements into one line, and automatically appends each new element. List comprehensions are not always presented to beginners, but I have included them here because you’ll most likely see them as soon as you start looking at other people’s code.

The following example builds the same list of square numbers you saw earlier but uses a list comprehension:

```{code-cell} ipython3
squares = [value**2 for value in range(1, 11)]
print(squares)
```

To use this syntax, begin with a descriptive name for the list, such as squares. Next, open a set of square brackets and define the expression for the values you want to store in the new list. In this example the expression is value ** 2, which raises the value to the second power. Then, write a for loop to generate the numbers you want to feed into the expression, and close the square brackets. The for loop in this example is for value in range(1, 11), which feeds the values 1 through 10 into the expression value ** 2. Notice that no colon is used at the end of the for statement.

## Working with Part of a List

In Lecture 3 you learned how to access single elements in a list, and in this chapter you’ve been learning how to work through all the elements in a list. You can also work with a specific group of items in a list, which Python calls a slice.

### Slicing a List

To make a slice, you specify the index of the first and last elements you want to work with. As with the range() function, Python stops one item before the second index you specify. To output the first three elements in a list, you would request indices 0 through 3, which would return elements 0, 1, and 2. The following example involves a list of players on a team:

```{code-cell} ipython3
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[0:3])
```

```{code-cell} ipython3
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[1:4])
```

If you omit the first index in a slice, Python automatically starts your slice at the beginning of the list:

```{code-cell} ipython3
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[:4])
```

A similar syntax works if you want a slice that includes the end of a list. For example, if you want all items from the third item through the last item, you can start with index 2 and omit the second index:

```{code-cell} ipython3
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[2:])
```

This syntax allows you to output all of the elements from any point in your list to the end regardless of the length of the list. Recall that a negative index returns an element a certain distance from the end of a list; therefore, you can output any slice from the end of a list. For example, if we want to output the last three players on the roster, we can use the slice players[-3:]:

```{code-cell} ipython3
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[-3:])
```

### Looping Through a Slice

You can use a slice in a for loop if you want to loop through a subset of the elements in a list. In the next example we loop through the first three players and print their names as part of a simple roster:

```{code-cell} ipython3
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print("Here are the first three players on my team:")
for player in players[:3]:
    print(player.title())
```

### Copying a List

Often, you’ll want to start with an existing list and make an entirely new list based on the first one. Let’s explore how copying a list works and examine one situation in which copying a list is useful.

To copy a list, you can make a slice that includes the entire original list by omitting the first index and the second index ([:]). This tells Python to make a slice that starts at the first item and ends with the last item, producing a copy of the entire list.

For example, imagine we have a list of our favorite foods and want to make a separate list of foods that a friend likes. This friend likes everything in our list so far, so we can create their list by copying ours:

```{code-cell} ipython3
my_foods = ['pizza', 'falafel', 'carrot cake']
friend_foods = my_foods[:]
print("My favorite foods are:")
print(my_foods)
print("\nMy friend's favorite foods are:")
print(friend_foods)
```

To prove that we actually have two separate lists, we’ll add a new food to each list and show that each list keeps track of the appropriate person’s favorite foods:

```{code-cell} ipython3
my_foods = ['pizza', 'falafel', 'carrot cake']
friend_foods = my_foods[:]
my_foods.append('cannoli')
friend_foods.append('ice cream')
print("My favorite foods are:")
print(my_foods)
print("\nMy friend's favorite foods are:")
print(friend_foods)
```

```{code-cell} ipython3
my_foods = ['pizza', 'falafel', 'carrot cake']
# This doesn't work:
friend_foods = my_foods
my_foods.append('cannoli')
friend_foods.append('ice cream')
print("My favorite foods are:")
print(my_foods)
print("\nMy friend's favorite foods are:")
print(friend_foods)
```

## Tuples

Lists work well for storing collections of items that can change throughout the life of a program. The ability to modify lists is particularly important when you’re working with a list of users on a website or a list of characters in a game. However, sometimes you’ll want to create a list of items that cannot change. Tuples allow you to do just that. Python refers to values that cannot change as immutable, and an immutable list is called a tuple.

### Defining a Tuple

A tuple looks just like a list except you use parentheses instead of square brackets. Once you define a tuple, you can access individual elements by using each item’s index, just as you would for a list. For example, if we have a rectangle that should always be a certain size, we can ensure that its size doesn’t change by putting the dimensions into a tuple:

```{code-cell} ipython3
dimensions = (200, 50)
print(dimensions[0])
print(dimensions[1])
```

Let’s see what happens if we try to change one of the items in the tuple dimensions:

```{code-cell} ipython3
dimensions = (200, 50)
# dimensions[0] = 250
```

Tuples are technically defined by the presence of a comma; the parentheses make them look neater and more readable. If you want to define a tuple with one element, you need to include a trailing comma:

```{code-cell} ipython3
my_t = (3,)
```

### Looping Through All Values in a Tuple

You can loop over all the values in a tuple using a for loop, just as you did with a list:

```{code-cell} ipython3
dimensions = (200, 50)
for dimension in dimensions:
    print(dimension)
```

### Writing over a Tuple

Although you can’t modify a tuple, you can assign a new value to a variable that represents a tuple. So if we wanted to change our dimensions, we could redefine the entire tuple:

```{code-cell} ipython3
dimensions = (200, 50)
print("Original dimensions:")
for dimension in dimensions:
    print(dimension)

dimensions = (400, 100)
print("\nModified dimensions:")
for dimension in dimensions:
    print(dimension)
```
