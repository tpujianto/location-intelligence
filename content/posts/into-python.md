---
title: "Introduction to Python programming"
date: 2020-11-12T00:15:22-05:00
draft: false
disqus: false
---


In this first post related to Python programming I am going to start by answering perhaps the most common question asked by individuals new to writing code: "What can I do with Python?". The breadth of things you can do coding Python, is what makes the language typically appear in [top 5 programming languages for developers](https://insights.stackoverflow.com/survey/2019#most-popular-technologies). From creating a machine learning model that predicts building energy consumption, creating a stock trading platform, to developing desktop games, Python is a highly versatile, general purpose programming language used by many of the top global technology firms. This post will go over some of the fundamental concepts inherent to Python, that are essential to fully grasp later more complex tutorials.

There are a few key concepts that are common to all [object oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) languages, variables, operators, data structures, functions and classes. Thankfully, although these concepts might seem highly abstract at first, they are fairly easy to grasp in the Python language, as opposed to something like C++ or Scala. Each of the aforementioned concepts are outlined in sections below.


----

### Variables

A variable in python can be thought of as a bucket to contain data or as a way to attach a name to an object. And attaching a name to an object (or filling a bucket) is done with a simple "=" sign.

```
a = 'book'
```

In the above example we assign the value 'book' to an object called *a*. Now *a* can be referenced anywhere elsewhere in the code to retrieve it's value.

```
print(a)
'book'
```

Here we used the print function to view whats inside *a*.

Variables are mutable, which means they can be changed simply by assigning new values at any point in the code.

```
a = 'cover'
a = 'book cover'
print(a)
'book cover'
```

In the above example the value of *a* was first changed to 'cover', then to 'book cover', and as Python executes code procedurally from top to bottom, when the print function was made it printed the last value assigned to *a*.

### Data Structors

In the above section on variables, all the values assigned to *a* were words/text – and in Python this is a type of **data structure** called **strings**. Variables can take any value of within the Python family of data structures. Data Structures are the building blocks from which software is built. There are many types of data structures, to list a few: lists, dictionaries, tuples, integers, sets, etc. Below are brief desciptions of what they are and how they differ.

##### Lists

Lists can be thought of as containers to hold multiple objects simultaneously.

```
a = [1, 5, 7, 100]
```

Here *a* was assigned four numeric values. They are structured in a way that makes the values inside easily accessible later on in code.

```
print(a[0])
1
print(a[1])
5
```

Remembering that print can be used to reveal values inside a variable, in the above code *[0]* after *a* is a way to access the first element within *a*. If that number was changed to *[1]* it would print the second element in the list assigned to *a*. Lists are very powerful which we will see in later tutorials.



### Operators

bkosahjihadsfhdsjFHGDJIfhb
sakjFjSDBFKJhdgsjfgh



### Control Flow

bkosahjihadsfhdsjFHGDJIfhb
sakjFjSDBFKJhdgsjfgh
