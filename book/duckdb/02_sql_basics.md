---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.15.2
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# SQL Basics

## Introduction

This notebook is a short introduction to SQL. It is based on the [DuckDB](https://duckdb.org/) database engine.

## Datasets

The following datasets are used in this notebook. You don't need to download them, they can be accessed directly from the notebook.

- [cities.csv](https://open.gishub.org/data/duckdb/cities.csv)
- [countries.csv](https://open.gishub.org/data/duckdb/countries.csv)

## References

- [W3Schools SQL Tutorial](https://www.w3schools.com/sql)
- [DuckDB SQL Introduction](https://duckdb.org/docs/sql/introduction.html)

+++

## Installation

Uncomment the following cell to install the required packages.

```{code-cell} ipython3
# %pip install duckdb duckdb-engine jupysql
```

## Library Import and Configuration

```{code-cell} ipython3
import duckdb
import pandas as pd

# Import jupysql Jupyter extension to create SQL cells
%load_ext sql
```

Set configurations on jupysql to directly output data to Pandas and to simplify the output that is printed to the notebook.

```{code-cell} ipython3
%config SqlMagic.autopandas = True
%config SqlMagic.feedback = False
%config SqlMagic.displaycon = False
```

## Connecting to DuckDB

Connect jupysql to DuckDB using a SQLAlchemy-style connection string. You may either connect to an in memory DuckDB, or a file backed db.

```{code-cell} ipython3
%sql duckdb:///:memory:
# %sql duckdb:///path/to/file.db
```

 If your SQL query is one line only, you may use the `%sql` magic command. For multi-line SQL query, you may use the `%%sql` magic command. 

+++

## Install extensions

+++


Check available DuckDB extensions.

```{code-cell} ipython3
%%sql

SELECT * FROM duckdb_extensions();
```

DuckDB's [httpfs extension](https://duckdb.org/docs/extensions/httpfs) allows parquet and csv files to be queried remotely over http. This is useful for querying large datasets without having to download them locally. Let's install the extension and load the extension. 

```{code-cell} ipython3
%%sql

INSTALL httpfs;
LOAD httpfs;
```

## Read CSV

Use the `httpfs` extension to read the `cities.csv` file from the web. 

```{code-cell} ipython3
%%sql

SELECT * FROM 'https://open.gishub.org/data/duckdb/cities.csv';
```

```{code-cell} ipython3
%%sql

SELECT * FROM 'https://open.gishub.org/data/duckdb/countries.csv';
```

## Create Table

Create a table named `cities` from the `cities.csv` file.

```{code-cell} ipython3
%%sql 

CREATE TABLE cities AS SELECT * FROM 'https://open.gishub.org/data/duckdb/cities.csv';
```

Create a table named `countries` from the `countries.csv` file.

```{code-cell} ipython3
%%sql 

CREATE TABLE countries AS SELECT * FROM 'https://open.gishub.org/data/duckdb/countries.csv';
```

Display the table content in the database.

```{code-cell} ipython3
%%sql 

FROM cities;
```

```{code-cell} ipython3
%%sql 

FROM countries;
```

## The SQL SELECT statement

The `SELECT` statement is used to select data from a database. Use either `SELECT *` to select all columns, or `SELECT column1, column2, ...` to select specific columns.

`SELECT * FROM cities` is the same as `FROM cities`.

```{code-cell} ipython3
:tags: [hide-output]

%%sql 

SELECT * FROM cities;
```

To limit the number of rows returned, use the `LIMIT` keyword. For example, `SELECT * FROM cities LIMIT 10` will return only the first 10 rows.

```{code-cell} ipython3
%%sql

SELECT * FROM cities LIMIT 10;
```

Select a subset of columns from the `cities` table and display the first 10 rows.

```{code-cell} ipython3
%%sql

SELECT name, country FROM cities LIMIT 10;
```

To select distinct values, use the `DISTINCT` keyword. For example, `SELECT DISTINCT country FROM cities` will return only the distinct values of the `country` column.

```{code-cell} ipython3
%%sql

SELECT DISTINCT country FROM cities LIMIT 10;
```

To count the number of rows returned, use the `COUNT(*)` function. For example, `SELECT COUNT(*) FROM cities` will return the number of rows in the `cities` table.

```{code-cell} ipython3
%%sql

SELECT COUNT(*) FROM cities;
```

To count the number of distinct values, use the `COUNT(DISTINCT column)` function. For example, `SELECT COUNT(DISTINCT country) FROM cities` will return the number of distinct values in the `country` column.

```{code-cell} ipython3
%%sql

SELECT COUNT(DISTINCT country) FROM cities;
```

To calculate the maximum value, use the `MAX(column)` function. For example, `SELECT MAX(population) FROM cities` will return the maximum value in the `population` column.

```{code-cell} ipython3
%%sql

SELECT MAX(population) FROM cities;
```

To calculate the total value, use the `SUM(column)` function. For example, `SELECT SUM(population) FROM cities` will return the total value in the `population` column.

```{code-cell} ipython3
%%sql

SELECT SUM(population) FROM cities;
```

To calculate the average value, use the `AVG(column)` function. For example, `SELECT AVG(population) FROM cities` will return the average value in the `population` column.

```{code-cell} ipython3
%%sql

SELECT AVG(population) FROM cities;
```

To order the results, use the `ORDER BY column` clause. For example, `SELECT * FROM cities ORDER BY country` will return the rows ordered by the `country` column alphabetically.

```{code-cell} ipython3
%%sql

SELECT * FROM cities ORDER BY country LIMIT 10;
```

To order the results in descending order, use the `ORDER BY column DESC` clause. For example, `SELECT * FROM cities ORDER BY country ASC, population DESC` will return the rows ordered by the `country` column alphabetical order and then by the `population` column in descending order. 

```{code-cell} ipython3
%%sql 

SELECT * FROM cities ORDER BY country ASC, population DESC LIMIT 10;
```

## The WHERE Clause

The `WHERE` clause is used to filter records. The `WHERE` clause is used to extract only those records that fulfill a specified condition.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country='USA'
```

You can use boolean operators such as `AND`, `OR`, `NOT` to filter records. For example, `SELECT * FROM cities WHERE country='USA' OR country='CAN'` will return the rows where the `country` column is either `USA` or `CAN`.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country='USA' OR country='CAN';
```

To select US cities with a population greater than 1 million, use the following query: `SELECT * FROM cities WHERE country='USA' AND population > 1000000`.

```{code-cell} ipython3
:tags: [hide-output]

%%sql 

SELECT * FROM cities WHERE country='USA' AND population>1000000;
```

To select cities with the country name starting with the letter `U`, use the following query: `SELECT * FROM cities WHERE country LIKE 'U%'`.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country LIKE 'U%';
```

To select cities with the country name ending with the letter `A`, use the following query: `SELECT * FROM cities WHERE country LIKE '%A'`.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country LIKE '%A';
```

To select cities with the country name containing the letter `S` in the middle, use the following query: `SELECT * FROM cities WHERE country LIKE '_S_'`.

```{code-cell} ipython3
:tags: [hide-output]

%%sql 

SELECT * FROM cities WHERE country LIKE '_S_';
```

To select cities from a list of countries, use the `IN` operator. For example, `SELECT * FROM cities WHERE country IN ('USA', 'CAN')` will return the rows where the `country` column is either `USA` or `CAN`.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country IN ('USA', 'CAN');
```

To select cities with a population between 1 and 10 million, use the following query: `SELECT * FROM cities WHERE population BETWEEN 1000000 AND 10000000`.

```{code-cell} ipython3
:tags: [hide-output]

%%sql 

SELECT * FROM cities WHERE population BETWEEN 1000000 AND 10000000;
```

## SQL Joins

Reference: https://www.w3schools.com/sql/sql_join.asp

Here are the different types of the JOINs in SQL:

- `(INNER) JOIN`: Returns records that have matching values in both tables
- `LEFT (OUTER) JOIN`: Returns all records from the left table, and the matched records from the right table
- `RIGHT (OUTER) JOIN`: Returns all records from the right table, and the matched records from the left table
- `FULL (OUTER) JOIN`: Returns all records when there is a match in either left or right table

![](https://i.imgur.com/mITYzuS.png)

We have two sample tables: `cities` and `countries`.

There are 1,249 cities in the `cities` table and 243 countries in the `countries` table.

```{code-cell} ipython3
%%sql 

SELECT COUNT(*) FROM cities;
```

```{code-cell} ipython3
%%sql 

SELECT * FROM cities LIMIT 10;
```

```{code-cell} ipython3
%%sql 

SELECT COUNT(*) FROM countries;
```

```{code-cell} ipython3
%%sql 

SELECT * FROM countries LIMIT 10;
```

### SQL Inner Join

The `INNER JOIN` keyword selects records that have matching values in both tables. In the example, we join the `cities` table with the `countries` table using the `country` column in the `cities` table and the `Alpha3_code` column in the `countries` table. The result contains 1,244 rows, indicating that there are 5 cities that do not have a matching country.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities INNER JOIN countries ON cities.country = countries."Alpha3_code";
```

Only select the `city` and `country` columns from the `cities` table and the `Country` column from the `countries` table.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT name, cities."country", countries."Country" FROM cities INNER JOIN countries ON cities.country = countries."Alpha3_code";
```

### SQL Left Join

The `LEFT JOIN` keyword returns all records from the left table (`cities`), and the matched records from the right table (`countries`). The result contains 1,249 rows, the same number of rows as the `cities` table.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities LEFT JOIN countries ON cities.country = countries."Alpha3_code";
```

### SQL Right Join

The `RIGHT JOIN` keyword returns all records from the right table (`countries`), and the matched records from the left table (`cities`). The result contains 1,291 rows.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities RIGHT JOIN countries ON cities.country = countries."Alpha3_code";
```

### SQL Full Join

The `FULL JOIN` keyword returns all records when there is a match in either left or right table. The result contains 1,296 rows.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities FULL JOIN countries ON cities.country = countries."Alpha3_code";
```

### SQL Union

The `UNION` operator is used to combine the result-set of two or more `SELECT` statements. 

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT country FROM cities
UNION 
SELECT "Alpha3_code" FROM countries;
```

## Aggregation

### Group By

The `GROUP BY` statement groups rows that have the same values into summary rows, like "find the number of cities in each country".

The `GROUP BY` statement is often used with aggregate functions (`COUNT`, `MAX`, `MIN`, `SUM`, `AVG`) to group the result-set by one or more columns.

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT COUNT(name), country 
FROM cities 
GROUP BY country 
ORDER BY COUNT(name) DESC;
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT countries."Country", COUNT(name)
FROM cities
LEFT JOIN countries ON cities.country = countries."Alpha3_code"
GROUP BY countries."Country"
ORDER BY COUNT(name) DESC;
```

### Having

The `HAVING` clause was added to SQL because the `WHERE` keyword could not be used with aggregate functions.

For example, to select countries with more than 40 cities:

```{code-cell} ipython3
%%sql 

SELECT COUNT(name), country
FROM cities
GROUP BY country
HAVING COUNT(name) > 40
ORDER BY COUNT(name) DESC;
```

```{code-cell} ipython3
%%sql

SELECT countries."Country", COUNT(name)
FROM cities
LEFT JOIN countries ON cities.country = countries."Alpha3_code"
GROUP BY countries."Country"
HAVING COUNT(name) > 40
ORDER BY COUNT(name) DESC;
```

## Conditional statements

The `CASE` statement goes through conditions and returns a value when the first condition is met (like an `IF-THEN-ELSE` statement). So, once a condition is true, it will stop reading and return the result. If no conditions are true, it returns the value in the `ELSE` clause.

For example, to divide cities into 3 groups based on their population:

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT name, population,
CASE
    WHEN population > 10000000 THEN 'Megacity'
    WHEN population > 1000000 THEN 'Large city'
    ELSE 'Small city'
END AS category
FROM cities;
```

## Saving results

You can save the results of a query to a new table using the `CREATE TABLE AS` statement.

```{code-cell} ipython3
%%sql

CREATE TABLE cities2 AS SELECT * FROM cities;
```

Show the new table content.

```{code-cell} ipython3
%%sql

FROM cities2;
```

Use the `DROP TABLE` statement to delete the table.

```{code-cell} ipython3
%%sql

DROP TABLE IF EXISTS cities_usa;
CREATE TABLE cities_usa AS (SELECT * FROM cities WHERE country = 'USA');
```

```{code-cell} ipython3
%%sql

FROM cities_usa;
```

Use the `INSERT INTO` statement to insert rows into a table.

```{code-cell} ipython3
%%sql 

INSERT INTO cities_usa (SELECT * FROM cities WHERE country = 'CAN');
```

## SQL Comments

Comments are used to explain sections of SQL statements, or to prevent execution of SQL statements.


### Single line comMents

Single line comments start with --.

Any text between -- and the end of the line will be ignored (will not be executed).

The following example uses a single-line comment as an explanation:

```{code-cell} ipython3
%%sql

SELECT * FROM cities LIMIT 10 -- This is a comment;
```

### Multi-line comments

Multi-line comments start with `/*` and end with `*/`.

Any text between `/*` and `*/` will be ignored.

The following example uses a multi-line comment as an explanation:

```{code-cell} ipython3
%%sql

SELECT COUNT(name), country 
FROM cities 
/*
 * Adding Group by
 * Adding Order by
 */
GROUP BY country 
ORDER BY COUNT(name) DESC
LIMIT 10;
```
