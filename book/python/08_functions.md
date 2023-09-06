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

# Functions

```{contents}
:local:
:depth: 2
```

In this lecture you’ll learn to write functions, which are named blocks of code that are designed to do one specific job. When you want to perform a particular task that you’ve defined in a function, you call the function responsible for it. If you need to perform that task multiple times throughout your program, you don’t need to type all the code for the same task again and again; you just call the function dedicated to handling that task, and the call tells Python to run the code inside the function. You’ll find that using functions makes your programs easier to write, read, test, and fix.

In this lecture you’ll also learn ways to pass information to functions. You’ll learn how to write certain functions whose primary job is to display information and other functions designed to process data and return a value or set of values. Finally, you’ll learn to store functions in separate files called modules to help organize your main program files.

- [DEFINING A FUNCTION](#defining-a-function)
- [PASSING ARGUMENTS](#passing-arguments)
- [RETURN VALUES](#return-values)
- [PASSING A LIST](#passing-a-list)
- [PASSING AN ARBITRARY NUMBER OF ARGUMENTS](#passing-arbitrary-arguments)
- [SUMMARY](#summary)

## DEFINING A FUNCTION <a class='anchor' id='defining-a-function'></a>

Here’s a simple function named greet_user() that prints a greeting:

```
➊ def greet_user():
➋     """Display a simple greeting."""
➌     print("Hello!")

➍ greet_user()
```

This example shows the simplest structure of a function. The line at ➊ uses the keyword def to inform Python that you’re defining a function. This is the function definition, which tells Python the name of the function and, if applicable, what kind of information the function needs to do its job. The parentheses hold that information. In this case, the name of the function is greet_user(), and it needs no information to do its job, so its parentheses are empty. (Even so, the parentheses are required.) Finally, the definition ends in a colon.

Any indented lines that follow def greet_user(): make up the body of the function. The text at ➋ is a comment called a docstring, which describes what the function does. Docstrings are enclosed in triple quotes, which Python looks for when it generates documentation for the functions in your programs.

The line print("Hello!") ➌ is the only line of actual code in the body of this function, so greet_user() has just one job: print("Hello!").

When you want to use this function, you call it. A function call tells Python to execute the code in the function. To call a function, you write the name of the function, followed by any necessary information in parentheses, as shown at ➍. Because no information is needed here, calling our function is as simple as entering greet_user(). As expected, it prints Hello!:

```{code-cell} ipython3
def greet_user():
    """Display a simple greeting."""
    print("Hello!")

greet_user()
```

### Passing Information to a Function

Modified slightly, the function greet_user() can not only tell the user Hello! but also greet them by name. For the function to do this, you enter username in the parentheses of the function’s definition at def greet_user(). By adding username here you allow the function to accept any value of username you specify. The function now expects you to provide a value for username each time you call it. When you call greet_user(), you can pass it a name, such as 'jesse', inside the parentheses:

```{code-cell} ipython3
def greet_user(username):
    """Display a simple greeting."""
    print(f"Hello, {username.title()}!")

greet_user('jesse')
```

### Arguments and Parameters

In the preceding greet_user() function, we defined greet_user() to require a value for the variable username. Once we called the function and gave it the information (a person’s name), it printed the right greeting.

The variable username in the definition of greet_user() is an example of a parameter, a piece of information the function needs to do its job. The value 'jesse' in greet_user('jesse') is an example of an argument. An argument is a piece of information that’s passed from a function call to a function. When we call the function, we place the value we want the function to work with in parentheses. In this case the argument 'jesse' was passed to the function greet_user(), and the value was assigned to the parameter username.

## PASSING ARGUMENTS <a class='anchor' id='passing-arguments'></a>

Because a function definition can have multiple parameters, a function call may need multiple arguments. You can pass arguments to your functions in a number of ways. You can use positional arguments, which need to be in the same order the parameters were written; keyword arguments, where each argument consists of a variable name and a value; and lists and dictionaries of values. Let’s look at each of these in turn.

### Positional Arguments

When you call a function, Python must match each argument in the function call with a parameter in the function definition. The simplest way to do this is based on the order of the arguments provided. Values matched up this way are called positional arguments.

To see how this works, consider a function that displays information about pets. The function tells us what kind of animal each pet is and the pet’s name, as shown here:

```{code-cell} ipython3
def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet('hamster', 'harry')
```

### Multiple Function Calls

You can call a function as many times as needed. Describing a second, different pet requires just one more call to describe_pet():

```{code-cell} ipython3
def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet('hamster', 'harry')
describe_pet('dog', 'willie')
```

Calling a function multiple times is a very efficient way to work. The code describing a pet is written once in the function. Then, anytime you want to describe a new pet, you call the function with the new pet’s information. Even if the code for describing a pet were to expand to ten lines, you could still describe a new pet in just one line by calling the function again.

You can use as many positional arguments as you need in your functions. Python works through the arguments you provide when calling the function and matches each one with the corresponding parameter in the function’s definition.

### Order Matters in Positional Arguments

You can get unexpected results if you mix up the order of the arguments in a function call when using positional arguments:

```{code-cell} ipython3
def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet('harry', 'hamster')
```

In this function call we list the name first and the type of animal second. Because the argument 'harry' is listed first this time, that value is assigned to the parameter animal_type. Likewise, 'hamster' is assigned to pet_name. Now we have a “harry” named “Hamster”.

If you get funny results like this, check to make sure the order of the arguments in your function call matches the order of the parameters in the function’s definition.

### Keyword Arguments

A keyword argument is a name-value pair that you pass to a function. You directly associate the name and the value within the argument, so when you pass the argument to the function, there’s no confusion (you won’t end up with a harry named Hamster). Keyword arguments free you from having to worry about correctly ordering your arguments in the function call, and they clarify the role of each value in the function call.

Let’s rewrite pets.py using keyword arguments to call describe_pet():

```{code-cell} ipython3
def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet(animal_type='hamster', pet_name='harry')
```

The function describe_pet() hasn’t changed. But when we call the function, we explicitly tell Python which parameter each argument should be matched with. When Python reads the function call, it knows to assign the argument 'hamster' to the parameter animal_type and the argument 'harry' to pet_name. The output correctly shows that we have a hamster named Harry.

The order of keyword arguments doesn’t matter because Python knows where each value should go. The following two function calls are equivalent:

```{code-cell} ipython3
describe_pet(animal_type='hamster', pet_name='harry')
describe_pet(pet_name='harry', animal_type='hamster')
```

### Default Values

When writing a function, you can define a default value for each parameter. If an argument for a parameter is provided in the function call, Python uses the argument value. If not, it uses the parameter’s default value. So when you define a default value for a parameter, you can exclude the corresponding argument you’d usually write in the function call. Using default values can simplify your function calls and clarify the ways in which your functions are typically used.

For example, if you notice that most of the calls to describe_pet() are being used to describe dogs, you can set the default value of animal_type to 'dog'. Now anyone calling describe_pet() for a dog can omit that information:

```{code-cell} ipython3
def describe_pet(pet_name, animal_type='dog'):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet(pet_name='willie')
```

We changed the definition of describe_pet() to include a default value, 'dog', for animal_type. Now when the function is called with no animal_type specified, Python knows to use the value 'dog' for this parameter.

Note that the order of the parameters in the function definition had to be changed. Because the default value makes it unnecessary to specify a type of animal as an argument, the only argument left in the function call is the pet’s name. Python still interprets this as a positional argument, so if the function is called with just a pet’s name, that argument will match up with the first parameter listed in the function’s definition. This is the reason the first parameter needs to be pet_name.

To describe an animal other than a dog, you could use a function call like this:

```{code-cell} ipython3
describe_pet(pet_name='harry', animal_type='hamster')
```

Because an explicit argument for animal_type is provided, Python will ignore the parameter’s default value.

### Equivalent Function Calls

Because positional arguments, keyword arguments, and default values can all be used together, often you’ll have several equivalent ways to call a function. Consider the following definition for describe_pet() with one default value provided:

```
def describe_pet(pet_name, animal_type='dog'):
```

With this definition, an argument always needs to be provided for pet_name, and this value can be provided using the positional or keyword format. If the animal being described is not a dog, an argument for animal_type must be included in the call, and this argument can also be specified using the positional or keyword format.

All of the following calls would work for this function:

```{code-cell} ipython3
# A dog named Willie.
describe_pet('willie')
describe_pet(pet_name='willie')

# A hamster named Harry.
describe_pet('harry', 'hamster')
describe_pet(pet_name='harry', animal_type='hamster')
describe_pet(animal_type='hamster', pet_name='harry')
```

### Avoiding Argument Errors

When you start to use functions, don’t be surprised if you encounter errors about unmatched arguments. Unmatched arguments occur when you provide fewer or more arguments than a function needs to do its work. For example, here’s what happens if we try to call describe_pet() with no arguments:

```{code-cell} ipython3
def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

# describe_pet()
```

### RETURN VALUES <a class='anchor' id='return-values'></a>

A function doesn’t always have to display its output directly. Instead, it can process some data and then return a value or set of values. The value the function returns is called a return value. The return statement takes a value from inside a function and sends it back to the line that called the function. Return values allow you to move much of your program’s grunt work into functions, which can simplify the body of your program.

### Returning a Simple Value

Let’s look at a function that takes a first and last name, and returns a neatly formatted full name:

```{code-cell} ipython3
def get_formatted_name(first_name, last_name):
    """Return a full name, neatly formatted."""
    full_name = f"{first_name} {last_name}"
    return full_name.title()

musician = get_formatted_name('jimi', 'hendrix')
print(musician)
```

### Making an Argument Optional

Sometimes it makes sense to make an argument optional so that people using the function can choose to provide extra information only if they want to. You can use default values to make an argument optional.

For example, say we want to expand get_formatted_name() to handle middle names as well. A first attempt to include middle names might look like this:

```{code-cell} ipython3
def get_formatted_name(first_name, middle_name, last_name):
    """Return a full name, neatly formatted."""
    full_name = f"{first_name} {middle_name} {last_name}"
    return full_name.title()

musician = get_formatted_name('john', 'lee', 'hooker')
print(musician)
```

This function works when given a first, middle, and last name. The function takes in all three parts of a name and then builds a string out of them. The function adds spaces where appropriate and converts the full name to title case.

But middle names aren’t always needed, and this function as written would not work if you tried to call it with only a first name and a last name. To make the middle name optional, we can give the middle_name argument an empty default value and ignore the argument unless the user provides a value. To make get_formatted_name() work without a middle name, we set the default value of middle_name to an empty string and move it to the end of the list of parameters:

```{code-cell} ipython3
def get_formatted_name(first_name, last_name, middle_name=''):
    """Return a full name, neatly formatted."""
    if middle_name:
        full_name = f"{first_name} {middle_name} {last_name}"
    else:
        full_name = f"{first_name} {last_name}"
    return full_name.title()

musician = get_formatted_name('jimi', 'hendrix')
print(musician)

musician = get_formatted_name('john', 'hooker', 'lee')
print(musician)
```

### Returning a Dictionary

A function can return any kind of value you need it to, including more complicated data structures like lists and dictionaries. For example, the following function takes in parts of a name and returns a dictionary representing a person:

```{code-cell} ipython3
def build_person(first_name, last_name):
    """Return a dictionary of information about a person."""
    person = {'first': first_name, 'last': last_name}
    return person

musician = build_person('jimi', 'hendrix')
print(musician)
```

This function takes in simple textual information and puts it into a more meaningful data structure that lets you work with the information beyond just printing it. The strings 'jimi' and 'hendrix' are now labeled as a first name and last name. You can easily extend this function to accept optional values like a middle name, an age, an occupation, or any other information you want to store about a person. For example, the following change allows you to store a person’s age as well:

```{code-cell} ipython3
def build_person(first_name, last_name, age=None):
    """Return a dictionary of information about a person."""
    person = {'first': first_name, 'last': last_name}
    if age:
        person['age'] = age
    return person

musician = build_person('jimi', 'hendrix', age=27)
print(musician)
```

We add a new optional parameter age to the function definition and assign the parameter the special value None, which is used when a variable has no specific value assigned to it. You can think of None as a placeholder value. In conditional tests, None evaluates to False. If the function call includes a value for age, that value is stored in the dictionary. This function always stores a person’s name, but it can also be modified to store any other information you want about a person.

### Using a Function with a while Loop

You can use functions with all the Python structures you’ve learned about so far. For example, let’s use the get_formatted_name() function with a while loop to greet users more formally. Here’s a first attempt at greeting people using their first and last names:

```{code-cell} ipython3
def get_formatted_name(first_name, last_name):
    """Return a full name, neatly formatted."""
    full_name = f"{first_name} {last_name}"
    return full_name.title()

# # This is an infinite loop!
# while True:
#     print("\nPlease tell me your name:")
#     print("(enter 'q' at any time to quit)")

#     f_name = input("First name: ")
#     if f_name == 'q':
#         break

#     l_name = input("Last name: ")
#     if l_name == 'q':
#         break

#     formatted_name = get_formatted_name(f_name, l_name)
#     print(f"\nHello, {formatted_name}!")
```

### PASSING A LIST <a class='anchor' id='passing-a-list'></a>

You’ll often find it useful to pass a list to a function, whether it’s a list of names, numbers, or more complex objects, such as dictionaries. When you pass a list to a function, the function gets direct access to the contents of the list. Let’s use functions to make working with lists more efficient.

Say we have a list of users and want to print a greeting to each. The following example sends a list of names to a function called greet_users(), which greets each person in the list individually:

```{code-cell} ipython3
def greet_users(names):
    """Print a simple greeting to each user in the list."""
    for name in names:
        msg = f"Hello, {name.title()}!"
        print(msg)

usernames = ['hannah', 'ty', 'margot']
greet_users(usernames)
```

### Modifying a List in a Function

When you pass a list to a function, the function can modify the list. Any changes made to the list inside the function’s body are permanent, allowing you to work efficiently even when you’re dealing with large amounts of data.

Consider a company that creates 3D printed models of designs that users submit. Designs that need to be printed are stored in a list, and after being printed they’re moved to a separate list. The following code does this without using functions:

```{code-cell} ipython3
# Start with some designs that need to be printed.
unprinted_designs = ['phone case', 'robot pendant', 'dodecahedron']
completed_models = []

# Simulate printing each design, until none are left.
#  Move each design to completed_models after printing.
while unprinted_designs:
    current_design = unprinted_designs.pop()
    print(f"Printing model: {current_design}")
    completed_models.append(current_design)

# Display all completed models.
print("\nThe following models have been printed:")
for completed_model in completed_models:
    print(completed_model)
```

This program starts with a list of designs that need to be printed and an empty list called completed_models that each design will be moved to after it has been printed. As long as designs remain in unprinted_designs, the while loop simulates printing each design by removing a design from the end of the list, storing it in current_design, and displaying a message that the current design is being printed. It then adds the design to the list of completed models. When the loop is finished running, a list of the designs that have been printed is displayed.

We can reorganize this code by writing two functions, each of which does one specific job. Most of the code won’t change; we’re just making it more carefully structured. The first function will handle printing the designs, and the second will summarize the prints that have been made:

```{code-cell} ipython3
def print_models(unprinted_designs, completed_models):
    """
    Simulate printing each design, until none are left.
    Move each design to completed_models after printing.
    """
    while unprinted_designs:
        current_design = unprinted_designs.pop()
        print(f"Printing model: {current_design}")
        completed_models.append(current_design)

def show_completed_models(completed_models):
    """Show all the models that were printed."""
    print("\nThe following models have been printed:")
    for completed_model in completed_models:
        print(completed_model)

unprinted_designs = ['phone case', 'robot pendant', 'dodecahedron']
completed_models = []

print_models(unprinted_designs, completed_models)
show_completed_models(completed_models)
```

We set up a list of unprinted designs and an empty list that will hold the completed models. Then, because we’ve already defined our two functions, all we have to do is call them and pass them the right arguments. We call print_models() and pass it the two lists it needs; as expected, print_models() simulates printing the designs. Then we call show_completed_models() and pass it the list of completed models so it can report the models that have been printed. The descriptive function names allow others to read this code and understand it, even without comments.

This program is easier to extend and maintain than the version without functions. If we need to print more designs later on, we can simply call print_models() again. If we realize the printing code needs to be modified, we can change the code once, and our changes will take place everywhere the function is called. This technique is more efficient than having to update code separately in several places in the program.

This example also demonstrates the idea that every function should have one specific job. The first function prints each design, and the second displays the completed models. This is more beneficial than using one function to do both jobs. If you’re writing a function and notice the function is doing too many different tasks, try to split the code into two functions. Remember that you can always call a function from another function, which can be helpful when splitting a complex task into a series of steps.

### Preventing a Function from Modifying a List

Sometimes you’ll want to prevent a function from modifying a list. For example, say that you start with a list of unprinted designs and write a function to move them to a list of completed models, as in the previous example. You may decide that even though you’ve printed all the designs, you want to keep the original list of unprinted designs for your records. But because you moved all the design names out of unprinted_designs, the list is now empty, and the empty list is the only version you have; the original is gone. In this case, you can address this issue by passing the function a copy of the list, not the original. Any changes the function makes to the list will affect only the copy, leaving the original list intact.

You can send a copy of a list to a function like this:

```
function_name(list_name[:])
```

The slice notation [:] makes a copy of the list to send to the function.

```{code-cell} ipython3
def print_models(unprinted_designs, completed_models):
    """
    Simulate printing each design, until none are left.
    Move each design to completed_models after printing.
    """
    while unprinted_designs:
        current_design = unprinted_designs.pop()
        print(f"Printing model: {current_design}")
        completed_models.append(current_design)

def show_completed_models(completed_models):
    """Show all the models that were printed."""
    print("\nThe following models have been printed:")
    for completed_model in completed_models:
        print(completed_model)

unprinted_designs = ['phone case', 'robot pendant', 'dodecahedron']
completed_models = []

print_models(unprinted_designs[:], completed_models)
```

The function print_models() can do its work because it still receives the names of all unprinted designs. But this time it uses a copy of the original unprinted designs list, not the actual unprinted_designs list. The list completed_models will fill up with the names of printed models like it did before, but the original list of unprinted designs will be unaffected by the function.

Even though you can preserve the contents of a list by passing a copy of it to your functions, you should pass the original list to functions unless you have a specific reason to pass a copy. It’s more efficient for a function to work with an existing list to avoid using the time and memory needed to make a separate copy, especially when you’re working with large lists.

### PASSING AN ARBITRARY NUMBER OF ARGUMENTS <a class='anchor' id='passing-arbitrary-arguments'></a>

Sometimes you won’t know ahead of time how many arguments a function needs to accept. Fortunately, Python allows a function to collect an arbitrary number of arguments from the calling statement.

For example, consider a function that builds a pizza. It needs to accept a number of toppings, but you can’t know ahead of time how many toppings a person will want. The function in the following example has one parameter, \*toppings, but this parameter collects as many arguments as the calling line provides:

```{code-cell} ipython3
def make_pizza(*toppings):
    """Print the list of toppings that have been requested."""
    print(toppings)

make_pizza('pepperoni')
make_pizza('mushrooms', 'green peppers', 'extra cheese')
```

The asterisk in the parameter name `*toppings` tells Python to make an empty tuple called toppings and pack whatever values it receives into this tuple. The print() call in the function body produces output showing that Python can handle a function call with one value and a call with three values. It treats the different calls similarly. Note that Python packs the arguments into a tuple, even if the function receives only one value.

Now we can replace the print() call with a loop that runs through the list of toppings and describes the pizza being ordered:

```{code-cell} ipython3
def make_pizza(*toppings):
    """Summarize the pizza we are about to make."""
    print("\nMaking a pizza with the following toppings:")
    for topping in toppings:
        print(f"- {topping}")

make_pizza('pepperoni')
make_pizza('mushrooms', 'green peppers', 'extra cheese')
```

### Mixing Positional and Arbitrary Arguments

If you want a function to accept several different kinds of arguments, the parameter that accepts an arbitrary number of arguments must be placed last in the function definition. Python matches positional and keyword arguments first and then collects any remaining arguments in the final parameter.

For example, if the function needs to take in a size for the pizza, that parameter must come before the parameter `*toppings`:

```{code-cell} ipython3
def make_pizza(size, *toppings):
    """Summarize the pizza we are about to make."""
    print(f"\nMaking a {size}-inch pizza with the following toppings:")
    for topping in toppings:
        print(f"- {topping}")

make_pizza(16, 'pepperoni')
make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
```

### Using Arbitrary Keyword Arguments

Sometimes you’ll want to accept an arbitrary number of arguments, but you won’t know ahead of time what kind of information will be passed to the function. In this case, you can write functions that accept as many key-value pairs as the calling statement provides. One example involves building user profiles: you know you’ll get information about a user, but you’re not sure what kind of information you’ll receive. The function build_profile() in the following example always takes in a first and last name, but it accepts an arbitrary number of keyword arguments as well:

```{code-cell} ipython3
def build_profile(first, last, **user_info):
    """Build a dictionary containing everything we know about a user."""
    user_info['first_name'] = first
    user_info['last_name'] = last
    return user_info

user_profile = build_profile('albert', 'einstein',
                             location='princeton',
                             field='physics')
print(user_profile)
```

The definition of build_profile() expects a first and last name, and then it allows the user to pass in as many name-value pairs as they want. The double asterisks before the parameter `**user_info` cause Python to create an empty dictionary called user_info and pack whatever name-value pairs it receives into this dictionary. Within the function, you can access the key-value pairs in user_info just as you would for any dictionary.

You can mix positional, keyword, and arbitrary values in many different ways when writing your own functions. It’s useful to know that all these argument types exist because you’ll see them often when you start reading other people’s code. It takes practice to learn to use the different types correctly and to know when to use each type. For now, remember to use the simplest approach that gets the job done. As you progress you’ll learn to use the most efficient approach each time.

## SUMMARY <a class='anchor' id='summary'></a>

In this chapter you learned how to write functions and to pass arguments so that your functions have access to the information they need to do their work. You learned how to use positional and keyword arguments, and how to accept an arbitrary number of arguments. You saw functions that display output and functions that return values. You learned how to use functions with lists, dictionaries, if statements, and while loops.

One of your goals as a programmer should be to write simple code that does what you want it to, and functions help you do this. They allow you to write blocks of code and leave them alone once you know they work. When you know a function does its job correctly, you can trust that it will continue to work and move on to your next coding task.

Functions allow you to write code once and then reuse that code as many times as you want. When you need to run the code in a function, all you need to do is write a one-line call and the function does its job. When you need to modify a function’s behavior, you only have to modify one block of code, and your change takes effect everywhere you’ve made a call to that function.

Using functions makes your programs easier to read, and good function names summarize what each part of a program does. Reading a series of function calls gives you a much quicker sense of what a program does than reading a long series of code blocks.

Functions also make your code easier to test and debug. When the bulk of your program’s work is done by a set of functions, each of which has a specific job, it’s much easier to test and maintain the code you’ve written. You can write a separate program that calls each function and tests whether each function works in all the situations it may encounter. When you do this, you can be confident that your functions will work properly each time you call them.
