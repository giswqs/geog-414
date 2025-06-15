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

# Introduction to PostGIS

**Setting up the conda env:**

```
conda create -n sql python
conda activate sql
conda install ipython-sql sqlalchemy psycopg2 notebook pandas -c conda-forge
```

**Sample dataset:**
- [nyc_data.zip](https://github.com/giswqs/postgis/raw/master/data/nyc_data.zip) (Watch this [video](https://youtu.be/fROzLrjNDrs) to load data into PostGIS)

**References**:
- [Introduction to PostGIS](https://postgis.net/workshops/postgis-intro)
- [Using SQL with Geodatabases](https://desktop.arcgis.com/en/arcmap/latest/manage-data/using-sql-with-gdbs/sql-and-enterprise-geodatabases.htm)

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
database = "nyc"
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

SELECT * FROM nyc_neighborhoods WHERE FALSE
```

```{code-cell} ipython3
%%sql

SELECT id, boroname, name from nyc_neighborhoods LIMIT 10
```

## Simple SQL

```{code-cell} ipython3
%%sql

SELECT postgis_full_version()
```

### NYC Neighborhoods

![](https://i.imgur.com/eycL547.png)

+++

What are the names of all the neighborhoods in New York City?

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT name FROM nyc_neighborhoods
```

What are the names of all the neighborhoods in Brooklyn?

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT name
FROM nyc_neighborhoods
WHERE boroname = 'Brooklyn'
```

What is the number of letters in the names of all the neighborhoods in Brooklyn?

```{code-cell} ipython3
:tags: [hide-output]

%%sql

SELECT char_length(name)
FROM nyc_neighborhoods
WHERE boroname = 'Brooklyn'
```

What is the average number of letters and standard deviation of number of letters in the names of all the neighborhoods in Brooklyn?

```{code-cell} ipython3
%%sql

SELECT avg(char_length(name)), stddev(char_length(name))
FROM nyc_neighborhoods
WHERE boroname = 'Brooklyn'
```

What is the average number of letters in the names of all the neighborhoods in New York City, reported by borough?

```{code-cell} ipython3
%%sql

SELECT boroname, avg(char_length(name)), stddev(char_length(name))
FROM nyc_neighborhoods
GROUP BY boroname
```

### NYC Census Blocks

![](https://i.imgur.com/tHyMJMm.png)

```{code-cell} ipython3
%%sql

SELECT * FROM nyc_census_blocks WHERE FALSE
```

What is the population of the City of New York?

```{code-cell} ipython3
%%sql

SELECT Sum(popn_total) AS population
FROM nyc_census_blocks
```

What is the population of the Bronx?

```{code-cell} ipython3
%%sql

SELECT SUM(popn_total) AS population
FROM nyc_census_blocks
WHERE boroname = 'The Bronx'
```

For each borough, what percentage of the population is white?

```{code-cell} ipython3
%%sql

SELECT
boroname,
100 * SUM(popn_white)/SUM(popn_total) AS white_pct
FROM nyc_census_blocks
GROUP BY boroname
```
