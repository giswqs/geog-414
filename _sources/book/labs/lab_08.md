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

# Lab 8

**Firstname Lastname**

**Submission requirements**

1. An HTML version of your notebook (VS Code > Notebook > Export > HTML). The HTML file must show the output of your code.
2. A link to your notebook on Colab.

**Datasets**:

The following datasets are used in this lab. You don't need to download them manually, they can be accessed directly from the notebook.

- [nyc_subway_stations.tsv](https://open.gishub.org/data/duckdb/nyc_subway_stations.tsv)
- [nyc_neighborhoods.tsv](https://open.gishub.org/data/duckdb/nyc_neighborhoods.tsv)

```{code-cell} ipython3
# %pip install duckdb duckdb-engine jupysql
```

```{code-cell} ipython3
import duckdb

%load_ext sql
```

```{code-cell} ipython3
%config SqlMagic.autopandas = True
%config SqlMagic.feedback = False
%config SqlMagic.displaycon = False
```

## Question 1: Creating Tables

Create a database, then write a SQL query to create a table named `nyc_subway_stations` and load the data from the file `nyc_subway_stations.tsv` into it. Similarly, create a table named `nyc_neighborhoods` and load the data from the file `nyc_neighborhoods.tsv` into it.

```{code-cell} ipython3
# Add your code here.
```

## Question 2: Column Filtering

Write a SQL query to display the `ID`, `NAME`, and `BOROUGH` of each subway station in the `nyc_subway_stations` dataset.

```{code-cell} ipython3
# Add your code here.
```

## Question 3: Row Filtering

Write a SQL query to find all subway stations in the `nyc_subway_stations` dataset that are located in the borough of Manhattan.

```{code-cell} ipython3
# Add your code here.
```

## Question 4: Sorting Results

Write a SQL query to list the subway stations in the `nyc_subway_stations` dataset in alphabetical order by their names.

```{code-cell} ipython3
# Add your code here.
```

## Question 5: Unique Values

Write a SQL query to find the distinct boroughs represented in the `nyc_subway_stations` dataset.

```{code-cell} ipython3
# Add your code here.
```

## Question 6: Counting Rows

Write a SQL query to count the number of subway stations in each borough in the `nyc_subway_stations` dataset.

```{code-cell} ipython3
# Add your code here.
```

## Question 7: Aggregating Data

Write a SQL query to list the number of subway stations in each borough, sorted in descending order by the count.

```{code-cell} ipython3
# Add your code here.
```

## Question 8: Joining Tables

Write a SQL query to join the `nyc_subway_stations` and `nyc_neighborhoods` datasets on the borough name, displaying the subway station name and the neighborhood name.

```{code-cell} ipython3
# Add your code here.
```

## Question 9: String Manipulation

Write a SQL query to display the names of subway stations in the `nyc_subway_stations` dataset that contain the word "St" in their names.

```{code-cell} ipython3
# Add your code here.
```

## Question 10: Filtering with Multiple Conditions

Write a SQL query to find all subway stations in the `nyc_subway_stations` dataset that are in the borough of Brooklyn and have routes that include the letter "R".

```{code-cell} ipython3
# Add your code here.
```
