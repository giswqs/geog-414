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

# Lab 10

**Submission requirements**

1. **HTML Version:** Submit an HTML version of your notebook. Ensure all code outputs are visible. (Export via VS Code: Notebook > Export > HTML).
2. **Colab Link:** Provide a link to your notebook hosted on Google Colab for interactive review.

## Setup

Uncomment and run the following cell to install the required packages.

```{code-cell} ipython3
# %pip install duckdb leafmap lonboard
```

```{code-cell} ipython3
import duckdb
import leafmap
```

## Question 1

Download the [nyc_data.zip](https://github.com/opengeos/data/raw/main/duckdb/nyc_data.zip) dataset using leafmap. The zip file contains the following datasets. Create a new DuckDB database and import the datasets into the database. Each dataset should be imported into a separate table. 

- nyc_census_blocks
- nyc_homicides
- nyc_neighborhoods
- nyc_streets
- nyc_subway_stations

```{code-cell} ipython3
# Add your code here
```

## Question 2

+++

Visualize the `nyc_subway_stations` and `nyc_streets` datasets on the same map using leafmap and lonboard.

```{code-cell} ipython3
# Add your code here
```

## Question 3

Find out what neighborhood the `BLUE` subway stations are in.

```{code-cell} ipython3
# Add your code here
```

## Question 4

Find out what streets are within 200 meters of the `BLUE` subway stations.

```{code-cell} ipython3
# Add your code here
```

## Question 5

Visualize the `BLUE` subway stations and the streets within 200 meters of the `BLUE` subway stations on the same map using leafmap and lonboard.

```{code-cell} ipython3
# Add your code here
```
