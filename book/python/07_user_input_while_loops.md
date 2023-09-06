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

# Looping

```{contents}
:local:
:depth: 2
```

Most programs are written to solve an end user’s problem. To do so, you usually need to get some information from the user. For a simple example, let’s say someone wants to find out whether they’re old enough to vote. If you write a program to answer this question, you need to know the user’s age before you can provide an answer. The program will need to ask the user to enter, or input, their age; once the program has this input, it can compare it to the voting age to determine if the user is old enough and then report the result.

In this chapter you’ll learn how to accept user input so your program can then work with it. When your program needs a name, you’ll be able to prompt the user for a name. When your program needs a list of names, you’ll be able to prompt the user for a series of names. To do this, you’ll use the input() function.

You’ll also learn how to keep programs running as long as users want them to, so they can enter as much information as they need to; then, your program can work with that information. You’ll use Python’s while loop to keep programs running as long as certain conditions remain true.

With the ability to work with user input and the ability to control how long your programs run, you’ll be able to write fully interactive programs.

- [HOW THE INPUT() FUNCTION WORKS](#how-the-input-function-works)
- [INTRODUCING WHILE LOOPS](#introducing-while-loops)
- [USING A WHILE LOOP WITH LISTS AND DICTIONARIES](#using-while-loops)
- [SUMMARY](#summary)

## HOW THE INPUT() FUNCTION WORKS

The input() function pauses your program and waits for the user to enter some text. Once Python receives the user’s input, it assigns that input to a variable to make it convenient for you to work with.

For example, the following program asks the user to enter some text, then displays that message back to the user:

<!-- #endregion -->

```{code-cell} ipython3
# message = input("Tell me something, and I will repeat it back to you: ")
# print(message)
```

The input() function takes one argument: the prompt, or instructions, that we want to display to the user so they know what to do. In this example, when Python runs the first line, the user sees the prompt Tell me something, and I will repeat it back to you: . The program waits while the user enters their response and continues after the user presses ENTER. The response is assigned to the variable message, then print(message) displays the input back to the user:

## Writing Clear Prompts

Each time you use the input() function, you should include a clear, easy-to-follow prompt that tells the user exactly what kind of information you’re looking for. Any statement that tells the user what to enter should work. For example:

```{code-cell} ipython3
# name = input("Please enter your name: ")
# print(f"\nHello, {name}!")
```

Sometimes you’ll want to write a prompt that’s longer than one line. For example, you might want to tell the user why you’re asking for certain input. You can assign your prompt to a variable and pass that variable to the input() function. This allows you to build your prompt over several lines, then write a clean input() statement.

```{code-cell} ipython3
prompt = "If you tell us who you are, we can personalize the messages you see."
prompt += "\nWhat is your first name? "

# name = input(prompt)
# print(f"\nHello, {name}!")
```

This example shows one way to build a multi-line string. The first line assigns the first part of the message to the variable prompt. In the second line, the operator += takes the string that was assigned to prompt and adds the new string onto the end.

The prompt now spans two lines, again with space after the question mark for clarity:

## Using int() to Accept Numerical Input

When you use the input() function, Python interprets everything the user enters as a string. Consider the following interpreter session, which asks for the user’s age:

```{code-cell} ipython3
# age = input("How old are you? ")
# age
```

The user enters the number 21, but when we ask Python for the value of age, it returns '21', the string representation of the numerical value entered. We know Python interpreted the input as a string because the number is now enclosed in quotes. If all you want to do is print the input, this works well. But if you try to use the input as a number, you’ll get an error. We can resolve this issue by using the int() function, which tells Python to treat the input as a numerical value. The int() function converts a string representation of a number to a numerical representation, as shown here:

```{code-cell} ipython3
# age = input("How old are you? ")
# age = int(age)
# age
```

How do you use the int() function in an actual program? Consider a program that determines whether people are tall enough to ride a roller coaster:

```{code-cell} ipython3
# height = input("How tall are you, in inches? ")
# height = int(height)

# if height >= 48:
#     print("\nYou're tall enough to ride!")
# else:
#     print("\nYou'll be able to ride when you're a little older.")
```

## The Modulo Operator

A useful tool for working with numerical information is the modulo operator (%), which divides one number by another number and returns the remainder:

```{code-cell} ipython3
4 % 3
```

```{code-cell} ipython3
5 % 3
```

```{code-cell} ipython3
6 % 3
```

```{code-cell} ipython3
7 % 3
```

The modulo operator doesn’t tell you how many times one number fits into another; it just tells you what the remainder is.

When one number is divisible by another number, the remainder is 0, so the modulo operator always returns 0. You can use this fact to determine if a number is even or odd:

```{code-cell} ipython3
# number = input("Enter a number, and I'll tell you if it's even or odd: ")
# number = int(number)

# if number % 2 == 0:
#     print(f"\nThe number {number} is even.")
# else:
#     print(f"\nThe number {number} is odd.")
```

## INTRODUCING WHILE LOOPS

The for loop takes a collection of items and executes a block of code once for each item in the collection. In contrast, the while loop runs as long as, or while, a certain condition is true.

## The while Loop in Action

You can use a while loop to count up through a series of numbers. For example, the following while loop counts from 1 to 5:

```{code-cell} ipython3
current_number = 1
while current_number <= 5:
    print(current_number)
    current_number += 1
```

In the first line, we start counting from 1 by assigning current_number the value 1. The while loop is then set to keep running as long as the value of current_number is less than or equal to 5. The code inside the loop prints the value of current_number and then adds 1 to that value with current_number += 1. (The += operator is shorthand for current_number = current_number + 1.)

Python repeats the loop as long as the condition current_number <= 5 is true. Because 1 is less than 5, Python prints 1 and then adds 1, making the current number 2. Because 2 is less than 5, Python prints 2 and adds 1 again, making the current number 3, and so on. Once the value of current_number is greater than 5, the loop stops running and the program end.

The programs you use every day most likely contain while loops. For example, a game needs a while loop to keep running as long as you want to keep playing, and so it can stop running as soon as you ask it to quit. Programs wouldn’t be fun to use if they stopped running before we told them to or kept running even after we wanted to quit, so while loops are quite useful.

## Letting the User Choose When to Quit

We can make the parrot.py program run as long as the user wants by putting most of the program inside a while loop. We’ll define a quit value and then keep the program running as long as the user has not entered the quit value:

```{code-cell} ipython3
prompt = "\nTell me something, and I will repeat it back to you:"
prompt += "\nEnter 'quit' to end the program. "

message = ""
# while message != 'quit':
#     message = input(prompt)

#     if message != 'quit':
#         print(message)
```

The first time through the loop, message is just an empty string, so Python enters the loop. At message = input(prompt), Python displays the prompt and waits for the user to enter their input. Whatever they enter is assigned to message and printed; then, Python reevaluates the condition in the while statement. As long as the user has not entered the word 'quit', the prompt is displayed again and Python waits for more input. When the user finally enters 'quit', Python stops executing the while loop and the program end.

## Using a Flag

In the previous example, we had the program perform certain tasks while a given condition was true. But what about more complicated programs in which many different events could cause the program to stop running?

For example, in a game, several different events can end the game. When the player runs out of ships, their time runs out, or the cities they were supposed to protect are all destroyed, the game should end. It needs to end if any one of these events happens. If many possible events might occur to stop the program, trying to test all these conditions in one while statement becomes complicated and difficult.

For a program that should run only as long as many conditions are true, you can define one variable that determines whether or not the entire program is active. This variable, called a flag, acts as a signal to the program. We can write our programs so they run while the flag is set to True and stop running when any of several events sets the value of the flag to False. As a result, our overall while statement needs to check only one condition: whether or not the flag is currently True. Then, all our other tests (to see if an event has occurred that should set the flag to False) can be neatly organized in the rest of the program.

```{code-cell} ipython3
prompt = "\nTell me something, and I will repeat it back to you:"
prompt += "\nEnter 'quit' to end the program. "

active = True
# while active:
#     message = input(prompt)

#     if message == 'quit':
#         active = False
#     else:
#         print(message)
```

## Using break to Exit a Loop

To exit a while loop immediately without running any remaining code in the loop, regardless of the results of any conditional test, use the break statement. The break statement directs the flow of your program; you can use it to control which lines of code are executed and which aren’t, so the program only executes code that you want it to, when you want it to.

For example, consider a program that asks the user about places they’ve visited. We can stop the while loop in this program by calling break as soon as the user enters the 'quit' value:

```{code-cell} ipython3
prompt = "\nPlease enter the name of a city you have visited:"
prompt += "\n(Enter 'quit' when you are finished.) "

# while True:
#     city = input(prompt)

#     if city == 'quit':
#         break
#     else:
#         print(f"I'd love to go to {city.title()}!")
```

A loop that starts with while True will run forever unless it reaches a break statement. The loop in this program continues asking the user to enter the names of cities they’ve been to until they enter 'quit'. When they enter 'quit', the break statement runs, causing Python to exit the loop.

## Using continue in a Loop

Rather than breaking out of a loop entirely without executing the rest of its code, you can use the continue statement to return to the beginning of the loop based on the result of a conditional test. For example, consider a loop that counts from 1 to 10 but prints only the odd numbers in that range:

```{code-cell} ipython3
current_number = 0
while current_number < 10:
    current_number += 1
    if current_number % 2 == 0:
        continue

    print(current_number)
```

## Avoiding Infinite Loops

Every while loop needs a way to stop running so it won’t continue to run forever. For example, this counting loop should count from 1 to 5:

```{code-cell} ipython3
x = 1
while x <= 5:
    print(x)
    x += 1
```

But if you accidentally omit the line x += 1 (as shown next), the loop will run forever:

```{code-cell} ipython3
# # This loop runs forever!
# x = 1
# while x <= 5:
#     print(x)
```

Every programmer accidentally writes an infinite while loop from time to time, especially when a program’s loops have subtle exit conditions. If your program gets stuck in an infinite loop, press CTRL-C or just close the terminal window displaying your program’s output.

To avoid writing infinite loops, test every while loop and make sure the loop stops when you expect it to. If you want your program to end when the user enters a certain input value, run the program and enter that value. If the program doesn’t end, scrutinize the way your program handles the value that should cause the loop to exit. Make sure at least one part of the program can make the loop’s condition False or cause it to reach a break statement.

## USING A WHILE LOOP WITH LISTS AND DICTIONARIES <a class='anchor' id='using-while-loops'> </a>

So far, we’ve worked with only one piece of user information at a time. We received the user’s input and then printed the input or a response to it. The next time through the while loop, we’d receive another input value and respond to that. But to keep track of many users and pieces of information, we’ll need to use lists and dictionaries with our while loops.

A for loop is effective for looping through a list, but you shouldn’t modify a list inside a for loop because Python will have trouble keeping track of the items in the list. To modify a list as you work through it, use a while loop. Using while loops with lists and dictionaries allows you to collect, store, and organize lots of input to examine and report on later.

## Moving Items from One List to Another

Consider a list of newly registered but unverified users of a website. After we verify these users, how can we move them to a separate list of confirmed users? One way would be to use a while loop to pull users from the list of unconfirmed users as we verify them and then add them to a separate list of confirmed users. Here’s what that code might look like:

```{code-cell} ipython3
# Start with users that need to be verified,
#  and an empty list to hold confirmed users.
unconfirmed_users = ['alice', 'brian', 'candace']
confirmed_users = []

# Verify each user until there are no more unconfirmed users.
#  Move each verified user into the list of confirmed users.
while unconfirmed_users:
    current_user = unconfirmed_users.pop()

    print(f"Verifying user: {current_user.title()}")
    confirmed_users.append(current_user)

# Display all confirmed users.
print("\nThe following users have been confirmed:")
for confirmed_user in confirmed_users:
    print(confirmed_user.title())
```

## Removing All Instances of Specific Values from a List

In Chapter 3 we used remove() to remove a specific value from a list. The remove() function worked because the value we were interested in appeared only once in the list. But what if you want to remove all instances of a value from a list?

Say you have a list of pets with the value 'cat' repeated several times. To remove all instances of that value, you can run a while loop until 'cat' is no longer in the list, as shown here:

```{code-cell} ipython3
pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
print(pets)

while 'cat' in pets:
    pets.remove('cat')

print(pets)
```

## Filling a Dictionary with User Input

You can prompt for as much input as you need in each pass through a while loop. Let’s make a polling program in which each pass through the loop prompts for the participant’s name and response. We’ll store the data we gather in a dictionary, because we want to connect each response with a particular user:

```{code-cell} ipython3
responses = {}

# Set a flag to indicate that polling is active.
polling_active = True

# while polling_active:
#     # Prompt for the person's name and response.
#     name = input("\nWhat is your name? ")
#     response = input("Which mountain would you like to climb someday? ")

#     # Store the response in the dictionary.
#     responses[name] = response

#     # Find out if anyone else is going to take the poll.
#     repeat = input("Would you like to let another person respond? (yes/ no) ")
#     if repeat == 'no':
#         polling_active = False

# # Polling is complete. Show the results.
# print("\n--- Poll Results ---")
# for name, response in responses.items():
#     print(f"{name} would like to climb {response}.")
```

## SUMMARY

In this chapter you learned how to use input() to allow users to provide their own information in your programs. You learned to work with both text and numerical input and how to use while loops to make your programs run as long as your users want them to. You saw several ways to control the flow of a while loop by setting an active flag, using the break statement, and using the continue statement. You learned how to use a while loop to move items from one list to another and how to remove all instances of a value from a list. You also learned how while loops can be used with dictionaries.
