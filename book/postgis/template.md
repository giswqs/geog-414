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

# Title

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

SELECT * from nyc_subway_stations LIMIT 5
```
