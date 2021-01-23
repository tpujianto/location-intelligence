---
title: "Introduction to Python programming"
date: 2020-12-29T00:15:22-05:00
draft: false
disqus: false
---

![missing image](img/python_logo.png "missing image")



\
The most common question asked by individuals new to writing code is: "What can I do with Python?". The breadth of things you can do coding Python is what makes the language typically appear in [top 5 programming languages lists](https://insights.stackoverflow.com/survey/2019#most-popular-technologies). From creating a machine learning model that predicts building energy consumption, creating a stock trading platform, to developing desktop games, Python is a highly versatile, general purpose programming language used by many of the top global technology firms. This post will go over some of the fundamental concepts of Python, that are essential for understanding later, more complex tutorials.

This post will go through variables, operators, and data structures, concepts that are common to all [object oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) languages. Although these concepts might seem abstract at first, they are fairly easy to grasp in the Python language, as opposed to something like C++ or Scala.

The Jupyter Notebook of this tutorial lives [here](https://github.com/carlobailey/location-intelligence/blob/main/_notebooks/jupyter_intro.ipynb)


----

### Variables

\
A variable in python can be thought of as a bucket to contain data or as a way to attach a name to an object. Attaching a name to an object (or filling a bucket) is done with a simple "=" sign.

```
a = 'book'
```

In the above example we assign the value 'book' to an object called *a*. Now *a* can be referenced anywhere elsewhere in the code to retrieve it's value.

```
print(a)
'book'
```

Here we used the **print** function to view whats inside of *a*.


```
a = 'cover'
a = 'book cover'
print(a)
'book cover'
```

Variables are mutable, which means they can be changed simply by assigning new values at any point in the code. In the above example the value of *a* was first changed to 'cover', then to 'book cover', and as Python executes code procedurally from top to bottom, when the print function was made it printed the last value assigned to *a*.


----

### Data Structures
\
If variables are containers to hold values, then data structures can be thought of as the *type* of values variables can take. For example, in the above section on variables, all the values assigned to *a* ('book', 'cover', etc), are words/text – and in Python this is a type of **data structure** called **strings**. This next section will go through a few of the other types of data structures found in Python: lists, dictionaries, sets.

#### Lists

Lists are perhaps one of the most widely used data structures, particularly for those new to coding. Lists can be thought of as containers to hold multiple values simultaneously.

```
a = [1, 5, 7, 100]
```

Lists are created using squared brackets *[]* or via the *list()* function and everything inside a list is separated by commas. Above *a* was assigned four numeric values.

```
print(a[0])
1
print(a[1])
5
```

Lists are structured in a way that makes the values inside easily retrievable later on in code. Items in lists are ordered by indexes, a series of ordered numbers starting from 0. In the above example, putting *[0]* after *a* is a way to access the first element within *a*. If that number was changed to *[1]* it would print the second element in the list assigned to *a*. This operation is known as indexing, as it involves retrieving values based on their order in the list, their index.

There are many operations to manipulate the contents of a list. The below example demonstrates how to add items to the end of lists, how to reverse the order of a list, and how to discover the length of a list (the number of items in a list).

```
# Adding items to a list
a.append(205)
print(a)
[1, 5, 7, 100, 205]

# Reversing a lists order
a.reverse()
[205, 100, 7, 5, 1]

# Getting the number of items in a list
len(a)
5
```

In the above code, pound signs (#) were used to add comments. Comments in Python are parts of the code not executed my the computer. It is usually used by developers to add human readable notes to future readers.

#### Dictionaries

Dictionaries are another data type in Python that are used to hold what are called, key-value pairs. At the simplest level, a key-value pair is just two values, one of which you have designated to be a "key" and the other you have designated to be the "value".

```
a = {'building_name': 'Chrysler', 'address': '405 Lexington Av', 'height': 274}
```

In this example, we use a dictionary (key-value pairs) to define the attributes of a building – it's address, height, and name. Dictionaries are created using curly brackets *{}* or via the *dict()* function.

To retrieve an attribute value is as simple as calling the specific *key*.

```
print( a['address'] )
'405 Lexington Av'
```

Where dictionaries really shine however, is being able to hold attributes for a collection of objects.

```
{
  'building_name': 'Chrysler', 'address': '405 Lexington Av', 'height': 274,
  'building_name': 'Woolworth', 'address': '233 Broadway', 'height': 241,
  'building_name': 'Flatiron', 'address': '175 5th Ave', 'height': 86,
}
```

Here the previous dictionary of a single building was extended to hold attributes for multiple.

Dictionaries also allow for hierarchical (or nested) structuring of data.

```
{
  'city': 'New York': {
    'counties': ['Kings', 'Bronx', 'Queens', 'Richmond', 'New York']
  }
}
```

Here the *key* city is given the value 'New York' which in turn holds a dictionary of a collection of counties.


#### Sets

Another useful data structure are sets – which have very close similarities to the mathematical notion of a [set](https://en.wikipedia.org/wiki/Set_(mathematics)). Sets in Python are unordered collection of objects, but unlike lists, sets only contain distinct objects. Sets are created with curly brackets or with the *set()* function.


```
materials = {'timber', 'concrete', 'glass', 'steel'}
```

Sets are often used to remove duplicate elements or for mathematical operations like unions, intersections, difference, etc.

```
# To remove duplicates
a = ['timber', 'timber', 'concrete']

set(a)
['timber', 'concrete']

# To union / combine two sets
a = {'timber', 'concrete', 'glass', 'steel'}
b = {'brick', 'stone'}
a.union(b)
{'timber', 'concrete', 'glass', 'steel', 'brick', 'stone'}
```

----

### Operators
\
If variables are about assigning values to objects and data structures are the various forms said values can take, operators are methods in Python for manipulating those values. Python operators fall into the following categories: mathematical, comparison, logical, identity, bitwise, and sequence. This section covers mathematical, logical and comparison.

#### Mathematical

Arithmetic operators perform mathematical operations on numbers and symbols. As most of the symbols are familiar to grade school audiences, below are perhaps the less obvious operations.

```
# Exponentiation: **
4 ** 2
16

# Modulus: %
3 % 2
1

# Absolute value: abs()
abs(-10)
10
```

#### Comparison
As the name suggests, comparison operators are used to compare two values. Answering questions like: Are these two values the same? Which value is greater of the two? Are these values different?

```
# Are these values the same: ==
x == y

# Are these values not the same: !=
x != y

# Greater than or equal to: >=
x >= y

# Less than or equal to: <=
x <= y
```

#### Logical
Logical operators test for truth and check whether specified conditions within a statement have been met. Answering questions like: Are both these statements true?

```
# Checking whether two statement are true: and
a == b and x == y

# Check whether one of the statements are true: or
a == b or x == y
```
