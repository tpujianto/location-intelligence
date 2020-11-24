---
title: "Querying Data with SQL"
date: 2020-11-22T17:16:09-05:00
draft: false
---

TO DO:
- joining data


Querying data, as the name suggests, is the process of asking a database for data in a specific format, combination of attributes, or combination of tables. The main programming language to do this is SQL (Structured Query Language). The SQL language has been in use for over forty years and is still he main gateway to retrieving vast amounts of information from databases. This post will walk through the fundamentals of exploring and analyzing data using SQL.

---

### Why SQL?
/
In the previous post Pandas was shown to be a great general purpose tool to manipulate data using Python. So a natural question could be, Why use SQL at all? Well there are a number of reasons SQL trumps Python/Pandas as a query language for data manipulation, and the main one is scalability. Python is great for doing analysis and modelling of small datasets ("small" is relative to the memory limits of the machine used), but SQL and modern database management systems are built for large scale operations. Another reason is that SQL syntax is very intuitive, and so it empowers more people beyond software engineers and data scientists to interact with data indepentant of technical capabilityes.

---

### Querying data
/
When retrieving data from a database using SQL, pretty much all statements (in SQL, sode is often referred to as statements or clauses) start with a **SELECT** keyword, followed by the columns of interest plus a **FROM** keyword. As an example, below is a table called *zipcodes* in the [geographies database].

The table has 5 columns:
- region_id – the zipcode identifier
- geom – the geometry in WKT format
- ZCTA5CE10 – tigerline identifier
- ALAND – the total land area
- AWATER – the total water area

**region_id**|**geom**|**ZCTA5CE10**|**ALAND**|**AWATER**
:-----:|:-----:|:-----:|:-----:|:-----:
53922|POLYGON ((-88.79170 43.53140, -88.79127 43.531...|53922|118603010|1919478
15027|POLYGON ((-80.25520 40.67407, -80.25538 40.673...|15027|3728900|473269

To retrieve all the columns in the table:

```
SELECT *
FROM geographies.zipcodes
LIMIT 10
```

The above query asks to retrieve all (**SELECT \***) columns from the *geographies.zipcodes* table. It **LIMIT**s the number of rows returned to 10 – using limits is considered best practice to avoid downloading unneccessaryily large amounts of data.

To retrieve only the region_id and AWATER columns:

```
SELECT region_id, aland
FROM geographies.zipcodes
LIMIT 10
```

Which would result in the below table:

**region\_id**|**AWATER**
:-----:|:-----:
53922|1919478
15027|473269

---

### Filtering data
\
SQL allows for the ability to filter data, by the values or attributes within a column or set of columns. This is done by adding a **WHERE** clause to the end of a query. For example, to retrieve zip codes that have an area of water above 1,900,000:

```
SELECT region_id
FROM geographies.zipcodes
WHERE awater > 1900000
```

To add more than one condition, statements can be chained together with **AND** clauses. For example, to retrieve zip codes that have an area of water above 1,900,000 and land area below 3,000,000:

```
SELECT region_id FROM geographies.zipcodes
WHERE awater > 1900000
AND aland < 3000000
```

---

### Joining data
\
Perhaps the most important concept within SQL and data analysis in general is the process of merging data/tables. In SQL this is known as **JOIN**s. There are various types of joins but they all involve matching rows between tables based on a common column/attribute. Below is an example of an **INNER JOIN**, one of the most common.

Say there are two tables in a database, a table of grocery items called **groceries** and a table of customers called **customers**.

**item_name**|**item\_id**
:-----:|:-----:
apples|001
oranges|002
plums|004
onions|010
cookies|021

---

**customer\_name**|**quantity**|**item\_id**
:-----:|:-----:|:-----:
Ashley|2|002
Ashley|1|021
Jo|3|001
Jo|1|004
Ashley|4|001

An INNER JOIN would be employed to match rows between these tables based on a common column, which in the above case is **item_id**. The SQL syntax is fairly straight forward in that after the aforementioned SELECT and FROM statements, follow an INNER JOIN statement, specifying the table to join and the column to join on.

```
SELECT item_name, customer_name, quantity
FROM groceries g
INNER JOIN customers c
ON g.item_id = c.item_id
```

This would result in a new table:

**items**|**customer\_name**|**quantity**
:-----:|:-----:|:-----:
apples|Ashley|2
oranges|Ashley|1
plums|Jo|3
onions|Jo|1
cookies|Ashley|4
