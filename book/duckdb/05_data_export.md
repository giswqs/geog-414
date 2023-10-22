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

## Datasets

The following datasets are used in this notebook. You don't need to download them, they can be accessed directly from the notebook.

- [cities.csv](https://open.gishub.org/data/duckdb/cities.csv)
- [countries.csv](https://open.gishub.org/data/duckdb/countries.csv)

## Installation

Uncomment the following cell to install the required packages if needed.

```{code-cell} ipython3
# %pip install duckdb duckdb-engine jupysql
```

## Library Import

```{code-cell} ipython3
import duckdb
import pandas as pd
```

## Installing Extensions

DuckDB’s Python API provides functions for installing and loading extensions, which perform the equivalent operations to running the `INSTALL` and `LOAD` SQL commands, respectively. An example that installs and loads the [httpfs extension](https://duckdb.org/docs/extensions/httpfs) looks like follows:
