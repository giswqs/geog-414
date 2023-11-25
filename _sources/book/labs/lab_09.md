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

# Lab 9

In this lab, you will explore spatial data analysis using Python and DuckDB. You'll work with real-world datasets, ranging from global country statistics to specific building datasets. This will give you a practical understanding of handling, analyzing, and visualizing spatial data.

**Submission requirements**

1. **HTML Version:** Submit an HTML version of your notebook. Ensure all code outputs are visible. (Export via VS Code: Notebook > Export > HTML).
2. **Colab Link:** Provide a link to your notebook hosted on Google Colab for interactive review.

## Setup

Ensure you have DuckDB and Leafmap installed. Run the following command if needed:

```{code-cell} ipython3
# %pip install duckdb leafmap
```

```{code-cell} ipython3
import duckdb
import leafmap
```

## Question 1

Connect to a duckdb database and install the `httpfs` and `spatial` extensions

```{code-cell} ipython3
# Add your code here
```

## Question 2

Download the [Admin 0 – Countries](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/) vector dataset from Natural Earth using the `leafmap.download_file()` function.

```{code-cell} ipython3
# Add your code here
```

## Question 3

Create a new table in your database called `countries` and load the data from the downloaded country shapefile into it.

```{code-cell} ipython3
# Add your code here
```

Calculate the total population of all countries in the database using the `POP_EST` column.

```{code-cell} ipython3
# Add your code here
```

Show the top 10 countries with the largest population.

```{code-cell} ipython3
# Add your code here
```

Select countries in Europe with a population greater than 10 million and order them by population in descending order.

```{code-cell} ipython3
# Add your code here
```

Save the results of the previous query as a new table called `europe`.

```{code-cell} ipython3
# Add your code here
```

Export the `europe` table as a GeoJSON file.

```{code-cell} ipython3
# Add your code here
```

## Question 4

Create a table called `text_zones` and load the data from the [taxi_zones.parquet](https://beta.source.coop/cholmes/nyc-taxi-zones/taxi_zones.parquet) into it.

```{code-cell} ipython3
# Add your code here
```

Find out the unique values in the `borough` column and order them alphabetically.

```{code-cell} ipython3
# Add your code here
```

Export the `text_zones` table as a parquet file.

```{code-cell} ipython3
# Add your code here
```

## Question 5

Explore the [Google Open Buildings](https://beta.source.coop/cholmes/google-open-buildings/v2/geoparquet-admin1/) and select a country of your choice with relatively small number of buildings (i.e., small file size). Get the three character country code and replace `[COUNTRY_NAME]` in the following path with the country code. Use it to load all the parquet files for the selected country into a new table called `buildings`.

`s3://us-west-2.opendata.source.coop/google-research-open-buildings/v2/geoparquet-admin1/country=[COUNTRY_NAME]/*.parquet`

```{code-cell} ipython3
# Add your code here
```

Find out the number of buildings in the selected country.

```{code-cell} ipython3
# Add your code here
```

Find out the total area of all buildings in the selected country.

```{code-cell} ipython3
# Add your code here
```

Export the `buildings` table as a GeoPackage file.

```{code-cell} ipython3
# Add your code here
```
