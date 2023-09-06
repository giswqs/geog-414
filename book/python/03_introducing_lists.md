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

# Introducing List

```{contents}
:local:
:depth: 2
```

In this lecture and the next you’ll learn what lists are and how to start working with the elements in a list. Lists allow you to store sets of information in one place, whether you have just a few items or millions of items. Lists are one of Python’s most powerful features readily accessible to new programmers, and they tie together many important concepts in programming.

- [What is a List?](#what-is-a-list)
- [Changing, Adding, and Removing Elements](#changing-elements)
- [Organizing a List](#organizing-a-list)
- [Avoiding Index Errors When Working with Lists](#avoiding-index-errors)

## Type in Python

Python have a built-in method called as type which generally come in handy while figuring out the type of variable used in the program in the runtime.

If a single argument (object) is passed to type() built-in, it returns type of the given object. If three arguments (name, bases and dict) are passed, it returns a new type object.

```{code-cell} ipython3
x = 5
s = "geeksforgeeks"
y = [1,2,3]
print(type(x))
print(type(s))
print(type(y))
```

## What is a List?

A list is a collection of items in a particular order. You can make a list that includes the letters of the alphabet, the digits from 0–9, or the names of all the people in your family. You can put anything you want into a list, and the items in your list don’t have to be related in any particular way. Because a list usually contains more than one element, it’s a good idea to make the name of your list plural, such as letters, digits, or names. In Python, square brackets ([]) indicate a list, and individual elements in the list are separated by commas. Here’s a simple example of a list that contains a few kinds of bicycles:

```{code-cell} ipython3
bicycles = ['trek', 'cannondale', 'redline', 'specialized']
print(bicycles)
```

### Accessing Elements in a List

Accessing Elements in a List Lists are ordered collections, so you can access any element in a list by telling Python the position, or index, of the item desired. To access an element in a list, write the name of the list followed by the index of the item enclosed in square brackets. For example, let’s pull out the first bicycle in the list bicycles:

```{code-cell} ipython3
print(bicycles[0])
```

You can also use the string methods from Chapter 2 on any element in this list. For example, you can format the element 'trek' more neatly by using the title() method:

```{code-cell} ipython3
print(bicycles[0].title())
```

### Index Positions Start at 0, Not 1

Python considers the first item in a list to be at position 0, not position 1. This is true of most programming languages, and the reason has to do with how the list operations are implemented at a lower level. If you’re receiving unexpected results, determine whether you are making a simple off-by-one error.

The second item in a list has an index of 1. Using this counting sys tem, you can get any element you want from a list by subtracting one from its position in the list. For instance, to access the fourth item in a list, you request the item at index 3.

The following asks for the bicycles at index 1 and index 3:

```{code-cell} ipython3
print(bicycles[1])
print(bicycles[3])
```

Python has a special syntax for accessing the last element in a list. By ask ing for the item at index -1, Python always returns the last item in the list:

```{code-cell} ipython3
bicycles = ['trek', 'cannondale', 'redline', 'specialized']
print(bicycles[-1])
```

### Using Individual Values from a List

You can use individual values from a list just as you would any other variable. For example, you can use f-strings to create a message based on a value from a list. Let’s try pulling the first bicycle from the list and composing a message using that value.

```{code-cell} ipython3
bicycles = ['trek', 'cannondale', 'redline', 'specialized']
message = f"My first bicycle was a {bicycles[0].title()}."
print(message)
```

## Changing, Adding, and Removing Elements

Most lists you create will be dynamic, meaning you’ll build a list and then add and remove elements from it as your program runs its course. For example, you might create a game in which a player has to shoot aliens out of the sky. You could store the initial set of aliens in a list and then remove an alien from the list each time one is shot down. Each time a new alien appears on the screen, you add it to the list. Your list of aliens will increase and decrease in length throughout the course of the game.

### Modifying Elements in a List

The syntax for modifying an element is similar to the syntax for accessing an element in a list. To change an element, use the name of the list followed by the index of the element you want to change, and then provide the new value you want that item to have.
For example, let’s say we have a list of motorcycles, and the first item in the list is 'honda'. How would we change the value of this first item?

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki']
print(motorcycles)
```

```{code-cell} ipython3
motorcycles[0] = 'ducati'
print(motorcycles)
```

### Adding Elements to a List

You might want to add a new element to a list for many reasons. For example, you might want to make new aliens appear in a game, add new data to a visualization, or add new registered users to a website you’ve built. Python provides several ways to add new data to existing lists.

#### Appending Elements to the End of a List

The simplest way to add a new element to a list is to append the item to the list. When you append an item to a list, the new element is added to the end of the list. Using the same list we had in the previous example, we’ll add the new element 'ducati' to the end of the list:

```{code-cell} ipython3
motorcycles.append('ducati')
print(motorcycles)
```

The append() method makes it easy to build lists dynamically. For example, you can start with an empty list and then add items to the list using a series of append() calls. Using an empty list, let’s add the elements 'honda', 'yamaha', and 'suzuki' to the list:

```{code-cell} ipython3
motorcycles = []

motorcycles.append('honda')
motorcycles.append('yamaha')
motorcycles.append('suzuki')

print(motorcycles)
```

### Inserting Elements into a List

You can add a new element at any position in your list by using the insert() method. You do this by specifying the index of the new element and the value of the new item.

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki']
motorcycles.insert(0, 'ducati')
print(motorcycles)
```

### Removing Elements from a List

Often, you’ll want to remove an item or a set of items from a list. For example, when a player shoots down an alien from the sky, you’ll most likely want to remove it from the list of active aliens. Or when a user decides to cancel their account on a web application you created, you’ll want to remove that user from the list of active users. You can remove an item according to its position in the list or according to its value. Removing an Item Using the del Statement If you know the position of the item you want to remove from a list, you can use the del statement.

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki']
print(motorcycles)
del motorcycles[0]
print(motorcycles)
```

```{code-cell} ipython3
del motorcycles[1]
print(motorcycles)
```

### Removing an Item Using the pop() Method

Sometimes you’ll want to use the value of an item after you remove it from a list. For example, you might want to get the x and y position of an alien that was just shot down, so you can draw an explosion at that position. In a web application, you might want to remove a user from a list of active members and then add that user to a list of inactive members. The pop() method removes the last item in a list, but it lets you work with that item after removing it. The term pop comes from thinking of a list as a stack of items and popping one item off the top of the stack. In this analogy, the top of a stack corresponds to the end of a list.

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki']
print(motorcycles)
popped_motorcycle = motorcycles.pop()
print(motorcycles)
print(popped_motorcycle)
```

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki']
last_owned = motorcycles.pop()
print(f"The last motorcycle I owned was a {last_owned.title()}.")
```

### Popping Items from any Position in a List

You can use pop() to remove an item from any position in a list by including the index of the item you want to remove in parentheses.

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki']
first_owned = motorcycles.pop(0)
print(f"The first motorcycle I owned was a {first_owned.title()}.")
```

```{code-cell} ipython3
print(motorcycles)
```

### Removing an Item by Value

Sometimes you won’t know the position of the value you want to remove from a list. If you only know the value of the item you want to remove, you can use the remove() method. For example, let’s say we want to remove the value 'ducati' from the list of motorcycles.

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki', 'ducati']
print(motorcycles)
motorcycles.remove('ducati')
print(motorcycles)
```

You can also use the remove() method to work with a value that’s being removed from a list. Let’s remove the value 'ducati' and print a reason for removing it from the list:

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki', 'ducati']
print(motorcycles)
too_expensive = 'ducati'
motorcycles.remove(too_expensive)
print(motorcycles)
print(f"\nA {too_expensive.title()} is too expensive for me.")
```

## Organizing a List

Often, your lists will be created in an unpredictable order, because you can’t always control the order in which your users provide their data. Although this is unavoidable in most circumstances, you’ll frequently want to present your information in a particular order. Sometimes you’ll want to preserve the original order of your list, and other times you’ll want to change the original order. Python provides a number of different ways to organize your lists, depending on the situation.

### Sorting a List Permanently with the sort() Method

Python’s sort() method makes it relatively easy to sort a list. Imagine we have a list of cars and want to change the order of the list to store them alphabetically. To keep the task simple, let’s assume that all the values in the list are lowercase.

```{code-cell} ipython3
cars = ['bmw', 'audi', 'toyota', 'subaru']
cars.sort()
print(cars)
```

You can also sort this list in reverse alphabetical order by passing the argument reverse=True to the sort() method. The following example sorts the list of cars in reverse alphabetical order:

```{code-cell} ipython3
cars = ['bmw', 'audi', 'toyota', 'subaru']
cars.sort(reverse=True)
print(cars)
```

### Sorting a List Temporarily with the sorted() Function

To maintain the original order of a list but present it in a sorted order, you can use the sorted() function. The sorted() function lets you display your list in a particular order but doesn’t affect the actual order of the list. Let’s try this function on the list of cars.

```{code-cell} ipython3
cars = ['bmw', 'audi', 'toyota', 'subaru']
print("Here is the original list:")
print(cars)
print("\nHere is the sorted list:")
print(sorted(cars))
print("\nHere is the original list again:")
print(cars)
```

### Printing a List in Reverse Order

To reverse the original order of a list, you can use the reverse() method. If we originally stored the list of cars in chronological order according to when we owned them, we could easily rearrange the list into reverse chronological order:

```{code-cell} ipython3
cars = ['bmw', 'audi', 'toyota', 'subaru']
print(cars)
cars.reverse()
print(cars)
```

### Finding the Length of a List

You can quickly find the length of a list by using the len() function. The list in this example has four items, so its length is 4:

```{code-cell} ipython3
cars = ['bmw', 'audi', 'toyota', 'subaru']
len(cars)
```

## Avoiding Index Errors When Working with Lists

One type of error is common to see when you’re working with lists for the first time. Let’s say you have a list with three items, and you ask for the fourth item:

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki']
# print(motorcycles[3])
```

Python attempts to give you the item at index 3. But when it searches the list, no item in motorcycles has an index of 3. Because of the off-by-one nature of indexing in lists, this error is typical. People think the third item is item number 3, because they start counting at 1. But in Python the third item is number 2, because it starts indexing at 0.

An index error means Python can’t find an item at the index you requested. If an index error occurs in your program, try adjusting the index you’re asking for by one. Then run the program again to see if the results are correct.

Keep in mind that whenever you want to access the last item in a list you use the index -1. This will always work, even if your list has changed size since the last time you accessed it:

```{code-cell} ipython3
motorcycles = ['honda', 'yamaha', 'suzuki']
print(motorcycles[-1])
```

```{code-cell} ipython3
motorcycles = []
# print(motorcycles[-1])
```
