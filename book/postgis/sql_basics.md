---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.14.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# SQL Basics

**Setting up the conda env:**

```
conda create -n sql python
conda activate sql
conda install ipython-sql sqlalchemy psycopg2 notebook pandas -c conda-forge
```

**Sample dataset:**

- [cities.csv](https://github.com/giswqs/postgis/blob/master/data/cities.csv)
- [countries.csv](https://raw.githubusercontent.com/giswqs/postgis/master/data/countries.csv)

+++

## Connecting to the database

```{code-cell} ipython3
%load_ext sql
```

```{code-cell} ipython3
import os
```

```{code-cell} ipython3
host = "localhost"
database = "sdb"
user = os.getenv('SQL_USER')
password = os.getenv('SQL_PASSWORD')
```

```{code-cell} ipython3
connection_string = f"postgresql://{user}:{password}@{host}/{database}"
```

```{code-cell} ipython3
%sql $connection_string
```

```{code-cell} ipython3
%%sql

SELECT * FROM cities LIMIT 10
```

## The SQL SELECT statement

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities
```

```{code-cell} ipython3
%%sql

SELECT * FROM cities LIMIT 10
```

```{code-cell} ipython3
%%sql

SELECT name, country FROM cities LIMIT 10
```

```{code-cell} ipython3
%%sql

SELECT DISTINCT country FROM cities LIMIT 10
```

```{code-cell} ipython3
%%sql

SELECT COUNT(DISTINCT country) FROM cities
```

```{code-cell} ipython3
%%sql

SELECT MAX(population) FROM cities
```

```{code-cell} ipython3
%%sql

SELECT SUM(population) FROM cities
```

```{code-cell} ipython3
%%sql

SELECT AVG(population) FROM cities
```

```{code-cell} ipython3
%%sql

SELECT * FROM cities ORDER BY country LIMIT 10
```

```{code-cell} ipython3
%%sql

SELECT * FROM cities ORDER BY country ASC, population DESC LIMIT 10
```

## The WHERE Clause

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country='USA'
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country='USA' OR country='CAN'
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country='USA' AND population>1000000
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country LIKE 'U%'
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country LIKE '%A'
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country LIKE '_S_'
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE country IN ('USA', 'CAN', 'CHN')
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities WHERE population BETWEEN 1000000 AND 10000000
```

## SQL Joins

Reference: https://www.w3schools.com/sql/sql_join.asp

Here are the different types of the JOINs in SQL:

- `(INNER) JOIN`: Returns records that have matching values in both tables
- `LEFT (OUTER) JOIN`: Returns all records from the left table, and the matched records from the right table
- `RIGHT (OUTER) JOIN`: Returns all records from the right table, and the matched records from the left table
- `FULL (OUTER) JOIN`: Returns all records when there is a match in either left or right table

![](https://i.imgur.com/mITYzuS.png)

```{code-cell} ipython3
%%sql

SELECT COUNT(*) FROM cities
```

```{code-cell} ipython3
%%sql

SELECT * FROM cities LIMIT 10
```

```{code-cell} ipython3
%%sql

SELECT COUNT(*) FROM countries
```

```{code-cell} ipython3
%%sql

SELECT * FROM countries LIMIT 10
```

### SQL Inner Join

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities INNER JOIN countries ON cities.country = countries."Alpha3_code"
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT name, country, countries."Country" FROM cities INNER JOIN countries ON cities.country = countries."Alpha3_code"
```

### SQL Left Join

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities LEFT JOIN countries ON cities.country = countries."Alpha3_code"
```

### SQL Right Join

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities RIGHT JOIN countries ON cities.country = countries."Alpha3_code"
```

### SQL Full Join

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT * FROM cities FULL JOIN countries ON cities.country = countries."Alpha3_code"
```

### SQL Union

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT country FROM cities
UNION
SELECT "Alpha3_code" FROM countries
```

## Aggregation

### Group By

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT COUNT(name), country
FROM cities
GROUP BY country
ORDER BY COUNT(name) DESC
```

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT countries."Country", COUNT(name)
FROM cities
LEFT JOIN countries ON cities.country = countries."Alpha3_code"
GROUP BY countries."Country"
ORDER BY COUNT(name) DESC
```

### Having

```{code-cell} ipython3
%%sql

SELECT COUNT(name), country
FROM cities
GROUP BY country
HAVING COUNT(name) > 40
ORDER BY COUNT(name) DESC
```

```{code-cell} ipython3
%%sql

SELECT countries."Country", COUNT(name)
FROM cities
LEFT JOIN countries ON cities.country = countries."Alpha3_code"
GROUP BY countries."Country"
HAVING COUNT(name) > 40
ORDER BY COUNT(name) DESC
```

## Conditional statements

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT name, population,
CASE
    WHEN population > 10000000 THEN 'Megacity'
    WHEN population > 1000000 THEN 'Large city'
    ELSE 'Small city'
END AS category
FROM cities
```

## Saving results

```{code-cell} ipython3
%%sql

SELECT *
INTO cities_new
FROM cities
```

```{code-cell} ipython3
%%sql

DROP TABLE IF EXISTS cities_usa;

SELECT *
INTO cities_usa
FROM cities
WHERE country = 'USA'
```

```{code-cell} ipython3
%%sql

INSERT INTO cities_usa
SELECT *
FROM cities
WHERE country = 'CAN'
```

## SQL Comments

### Single line comments

```{code-cell} ipython3
%%sql

SELECT * FROM cities LIMIT 10 -- This is a comment;
```

### Multi-line comments

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
LIMIT 10
```

```{code-cell} ipython3

```
