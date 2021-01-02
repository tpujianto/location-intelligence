---
title: "Introduction to Pandas"
date: 2020-12-28T23:21:50-05:00
draft: false
---


\
Pandas is a Python library for doing data analysis – it was built to make manipulating large datasets easy and fast. Initially built for use within the financial sector, it is now widely used throughout the data science and analytics industry. Some argue that it is the sole reason why Python is the de facto data science and machine learning language today.

Pandas relies heavily on the [Numpy](https://en.wikipedia.org/wiki/NumPy) library and C programming language, making it highly efficient. Pandas relies on two data structures that pretty much power nearly all other operations: *Series* and *DataFrames*. Understanding these are crucial to getting comfortable with the rest of the library's functionality.

This post will cover the basic concepts of DataFrames, Series, loading data, and merging data in Pandas.

---

### DataFrame
\
DataFrames can be thought of as spreadsheets or tables that contain rows and columns of data. Diving into the nuts and bolts of DataFrames is beyond the scope of this tutorial, but one thing to note is the idea of vectorization with Numpy and Pandas. Vectorization allows operations on large collections of values almost instantaneously without the need for running loops. And it is one of the main reasons Pandas is as fast as it is.

There are many ways to construct a DataFrame, one of the most common is to provide a dictionary with lists of equal length, inside the **DataFrame()** function.

```
data = {
  'column-1': [1, 2, 3, 4, 5]
  'column-2': ['a', 'b', 'c', 'd', 'e']
  'column-3': [2020, 2021, 2022, 2023, 2024]
}
```
**column-1**|**column-2**|**column-3**
:-----:|:-----:|:-----:
1|	a|	2020
2|	b|	2021
3|	c|	2022
4|	d|	2023
5|	e|	2024


To access a column's values is similar to calling values within a dictionary, e.g. with square brackets and column name. To access values of column 1 above would be.

```
data['column-1']
```
**column-1**
:-----:
1
2
3
4
5

To create a new column is as simple as passing a list or a single value.

```
data['column-4'] = ['apple', 'pear', 'grape', 'peach', 'kiwi']
data
```
**column-1**|**column-2**|**column-3**|**column-4**
:-----:|:-----:|:-----:|:-----:
1|	a|	2020| apple
2|	b|	2021| pear
3|	c|	2022| grape
4|	d|	2023| peach
5|	e|	2024| kiwi


---
### Series
\
Series can be thought of as column objects and a much smarter and faster version of lists.  

```
data = Series([1, 2, 3, 8])
```
| |
:-----:
1
2
3
8

Most Series consist of an array / list of values, alongside an associated list of labels, called *index*. Series are smarter than lists as you can access values either through specifying the indices or through index labels.

```
data = Series([1, 2, 3, 8], index=['a', 'b', 'c', 'd'])
```
| | |
:-----:|:-----:
a|1
b|2
c|3
d|8

```
data[1]
2

data.loc['a']
1
```

An important concept of Pandas (and Numpy) operations is that of vectorization – which is a programming paradigm that allows operations to be applied to an entire list of values without doing loops in Python.

```
data = Series([1, 2, 3, 8])
print(data * 2)
2
4
6
16
```

In the above example a Series called *data* is multiplied by 2, and the multiplication is applied to every value in the array. This is the same as scalar vector multiplication in linear algebra, so applies to any arithmetic operation with a single numeric value.

Pandas also makes it very easy to calculate aggregate or summary statistics of data within a Series or DataFrame.

```
data = Series([1, 2, 3, 8])

# calculating the total
data.sum()
14

# calculating averages
data.mean()
3.5
```

---

### Loading data from a CSV
\
Pandas has features to read many [different file types](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html) and data formats with a fairly simple syntax. To read a CSV file is as simple as:

```
read_csv('csv_file_name')
```

while reading JSON is:

```
read_json('json_file_name')
```

---

### Merging Datasets
\
The majority of data analysis and machine learning work is in finding, formatting, and joining across datasets. For example, things like satellite data are collected daily, and many provides of earth imagery data will provide it as a collection of hundreds (even thousands) of files. Pandas has functionality that makes it easy to combine data from multiple sources.

Datasets are combined by specifying a common key that is present in both. *Keys* here can be thought of as an attribute, column, or field. For example, two [time series](https://en.wikipedia.org/wiki/Time_series) datasets:

```
df1 = DataFrame({'column-a': [1, 2, 3, 4],
                 'key': ['a', 'b', 'c', 'd']})

df2 = DataFrame({'column-b': ['apple', 'pear', 'orange', 'kiwi'],
                 'key': ['a', 'b', 'c', 'd']})

print(df1)              print(df2)
    column-a  key           column-b  key
0   1         a         0   apple     a
1   2         b         1   pear      b
2   3         c         2   orange    c
3   4         d         3   kiwi      d
```

Can be combined using the **merge()** function and specifying the shared column:

```
df3 = merge(df1, df2, on='key')
print(df3)
    column-a  column-b  key
0   1         apple     a
1   2         pear      b
2   3         orange    c
3   4         kiwi      d
```

---

*Further Reading*:
- [Python For Data Analysis](https://www.cin.ufpe.br/~embat/Python%20for%20Data%20Analysis.pdf)
