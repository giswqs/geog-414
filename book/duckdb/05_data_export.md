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

# Data Export

## Introduction

This notebook demonstrates how to export data from the database to various formats, including Pandas DataFrames, CSV, JSON, Excel, Parquet, GeoJSON, Shapefile, and GeoPackage.

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
```

```{code-cell} ipython3
con.install_extension("httpfs")
con.load_extension("httpfs")
```

```{code-cell} ipython3
con.install_extension("spatial")
con.load_extension("spatial")
```

## Sample Data

```{code-cell} ipython3
con.sql(
    """
CREATE TABLE IF NOT EXISTS cities AS
SELECT * EXCLUDE geometry, ST_GeomFromWKB(geometry) 
AS geometry FROM 'https://open.gishub.org/data/duckdb/cities.parquet'
"""
)
```

```{code-cell} ipython3
con.table("cities").show()
```

## To DataFrames

```{code-cell} ipython3
con.table("cities").df()
```

## To CSV

```{code-cell} ipython3
con.sql("COPY cities TO 'cities.csv' (HEADER, DELIMITER ',')")
```

```{code-cell} ipython3
con.sql(
    "COPY (SELECT * FROM cities WHERE country='USA') TO 'cities_us.csv' (HEADER, DELIMITER ',')"
)
```

## To JSON

```{code-cell} ipython3
con.sql("COPY cities TO 'cities.json'")
```

```{code-cell} ipython3
con.sql("COPY (SELECT * FROM cities WHERE country='USA') TO 'cities_us.json'")
```

## To Excel

```{code-cell} ipython3
con.sql(
    "COPY (SELECT * EXCLUDE geometry FROM cities) TO 'cities.xlsx' WITH (FORMAT GDAL, DRIVER 'XLSX')"
)
```

## To Parquet

```{code-cell} ipython3
con.sql("COPY cities TO 'cities.parquet' (FORMAT PARQUET)")
```

```{code-cell} ipython3
con.sql(
    "COPY (SELECT * FROM cities WHERE country='USA') TO 'cities_us.parquet' (FORMAT PARQUET)"
)
```

## To GeoJSON

```{code-cell} ipython3
con.sql("COPY cities TO 'cities.geojson' WITH (FORMAT GDAL, DRIVER 'GeoJSON')")
```

```{code-cell} ipython3
con.sql(
    "COPY (SELECT * FROM cities WHERE country='USA') TO 'cities_us.geojson' WITH (FORMAT GDAL, DRIVER 'GeoJSON')"
)
```

## To Shapefile

Doens't work on Linux.

```{code-cell} ipython3
# con.sql("COPY cities TO 'cities.shp' WITH (FORMAT GDAL, DRIVER 'ESRI Shapefile')")
```

## To GeoPackage

```{code-cell} ipython3
con.sql("COPY cities TO 'cities.gpkg' WITH (FORMAT GDAL, DRIVER 'GPKG')")
```
