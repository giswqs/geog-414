---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.15.2
kernelspec:
  display_name: geo
  language: python
  name: python3
---

# Python API

## Introduction

There are various client APIs for DuckDB. DuckDB’s “native” API is C++, with “official” wrappers available for C, Python, R, Java, Node.js, WebAssembly/Wasm, ODBC API, Julia, and a Command Line Interface (CLI).

In this notebook, we will explore the [DuckDB Python API](https://duckdb.org/docs/api/python/overview).

## Datasets

The following datasets are used in this notebook. You don't need to download them, they can be accessed directly from the notebook.

- [cities.csv](https://open.gishub.org/data/duckdb/cities.csv)
- [countries.csv](https://open.gishub.org/data/duckdb/countries.csv)

## Installation

Uncomment the following cell to install the required packages if needed.

```{code-cell} ipython3
# %pip install duckdb
```

## Library Import

```{code-cell} ipython3
import duckdb
import pandas as pd
```

## Installing Extensions

DuckDB’s Python API provides functions for installing and loading extensions, which perform the equivalent operations to running the `INSTALL` and `LOAD` SQL commands, respectively. An example that installs and loads the [httpfs extension](https://duckdb.org/docs/extensions/httpfs) looks like follows:

```{code-cell} ipython3
con = duckdb.connect()
con.install_extension("httpfs")
con.load_extension("httpfs")
```

## Data Input

DuckDB can ingest data from a wide variety of formats – both on-disk and in-memory. See the [data ingestion page](https://duckdb.org/docs/api/python/data_ingestion) for more information.

```{code-cell} ipython3
con.sql('SELECT 42').show()
```

```{code-cell} ipython3
con.read_csv('https://open.gishub.org/data/duckdb/cities.csv')
```

```{code-cell} ipython3
con.read_csv('https://open.gishub.org/data/duckdb/countries.csv')
```

## DataFrames

DuckDB can also directly query Pandas DataFrames. 

```{code-cell} ipython3
pandas_df = pd.DataFrame({'a': [42]})
con.sql('SELECT * FROM pandas_df')
```

DuckDB can also ingest data from remote sources (e.g., HTTP, S3) and return a Pandas DataFrame.

```{code-cell} ipython3
df = con.read_csv('https://open.gishub.org/data/duckdb/cities.csv').df()
df.head()
```

## Result Conversion

DuckDB supports converting query results efficiently to a variety of formats. See the [result conversion page](https://duckdb.org/docs/api/python/result_conversion) for more information.

```{code-cell} ipython3
con.sql('SELECT 42').fetchall()  # Python objects
```

```{code-cell} ipython3
con.sql('SELECT 42').df()  # Pandas DataFrame
```

```{code-cell} ipython3
con.sql('SELECT 42').fetchnumpy()  # NumPy Arrays
```

## Writing Data to Disk

DuckDB supports writing Relation objects directly to disk in a variety of formats. The [COPY](https://duckdb.org/docs/sql/statements/copy) statement can be used to write data to disk using SQL as an alternative.

```{code-cell} ipython3
con.sql('SELECT 42').write_parquet('out.parquet')  # Write to a Parquet file
con.sql('SELECT 42').write_csv('out.csv')  # Write to a CSV file
con.sql("COPY (SELECT 42) TO 'out.parquet'")  # Copy to a parquet file
```

## Persistent Storage

By default DuckDB operates on an **in-memory** database. That means that any tables that are created are not persisted to disk. Using the `.connect` method a connection can be made to a persistent database. Any data written to that connection will be persisted, and can be reloaded by re-connecting to the same file.

```{code-cell} ipython3
# create a connection to a file called 'file.db'
con = duckdb.connect('file.db')
# create a table and load data into it
con.sql(
    'CREATE TABLE IF NOT EXISTS cities AS FROM read_csv_auto("https://open.gishub.org/data/duckdb/cities.csv")'
)
# query the table
con.table('cities').show()
# Note: connections also closed implicitly when they go out of scope
```

```{code-cell} ipython3
# explicitly close the connection
con.close()
```

You can also use a context manager to ensure that the connection is closed:

```{code-cell} ipython3
with duckdb.connect('file.db') as con:
    con.sql(
        'CREATE TABLE IF NOT EXISTS cities AS FROM read_csv_auto("https://open.gishub.org/data/duckdb/cities.csv")'
    )
    con.table('cities').show()
    # the context manager closes the connection automatically
```

## Connection Object and Module

The connection object and the `duckdb` module can be used interchangeably – they support the same methods. The only difference is that when using the `duckdb` module a global in-memory database is used.

Note that if you are developing a package designed for others to use, and use DuckDB in the package, it is recommend that you create connection objects instead of using the methods on the `duckdb` module. That is because the `duckdb` module uses a shared global database – which can cause hard to debug issues if used from within multiple different packages.

```{code-cell} ipython3
duckdb.sql('SELECT 42')
```

```{code-cell} ipython3
con = duckdb.connect()
con.sql('SELECT 42')
```

## References

- [DuckDB Python API Overview](https://duckdb.org/docs/api/python/overview)
