---
title: "Introduction to Pandas"
date: 2020-11-14T23:21:50-05:00
draft: false
---

TO DO:
- Add contents with links
- DataFrame
- Series
- Loading Data from CSV
- Combining and merging data
- Summarizing data

\
Pandas is a Python library for doing data analysis. It was built to make manipulating large datasets easy and fast. Initially built for use within the financial sector, it is now widely used throughout the data science and analytics industry. Some argue that it is the sole reason why Python is the de facto data science and machine learning language today.

Pandas relies heavily on the [Numpy](https://en.wikipedia.org/wiki/NumPy) library and C programming language, making it highly efficient. Pandas relies on two data structures that pretty much power nearly all other operations: *Series* and *DataFrames*. Understanding is crucial to getting comfortable with the rest of the library's functionality.

### DataFrame

DataFrames can be thought of as spreadsheets or tables that contain rows and columns of data. Diving into the nuts and bolts of DataFrames is beyond the scope of this tutorial, but one thing to note is the idea of vectorization with Numpy and Pandas. Vectorization allows operations on large collections of values almost instantaneously without the need for running loops. Amd it is one of the main reasons Pandas is as fast as it is.

There are many ways to construct a DataFrame but one of the most common is to provide a dictionary with lists of equal length.

```
data = {
  'column-1': [1, 2, 3, 4, 5]
  'column-2': ['a', 'b', 'c', 'd', 'e']
  'column-3': [2020, 2021, 2022, 2023, 2024]
}
```

To access a column's values is similar to calling values within a dictionary, e.g. with square brackets and column name. To access values of column 1 above would be.

```
data['column-1']
[1, 2, 3, 4, 5]
```

To create a new column is as simple as passing a list or a single value.

```
data['column-4'] = ['apple', 'pear', 'grape', 'peach', 'kiwi']
```

### Series


*Further Reading*:
- [Python For Data Analysis](https://www.cin.ufpe.br/~embat/Python%20for%20Data%20Analysis.pdf)
-
