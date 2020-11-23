---
title: "Querying Data with SQL"
date: 2020-11-22T17:16:09-05:00
draft: false
---

TO DO:
- filtering data
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
SELECT * FROM geographies.zipcodes LIMIT 10
```

The above query asks to retrieve all (**SELECT \***) columns from the *geographies.zipcodes* table. It **LIMIT**s the number of rows returned to 10 – using limits is considered best practice to avoid downloading unneccessaryily large amounts of data.

To retrieve only the region_id and AWATER columns:

```
SELECT region_id, aland FROM geographies.zipcodes LIMIT 10
```

Which would result in the below table:

**region\_id**|**AWATER**
:-----:|:-----:
53922|1919478
15027|473269

### Filtering data


### Joining data
