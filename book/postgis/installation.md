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

# Installation

**Setting up the conda env:**

```
conda create -n sql python
conda activate sql
conda install ipython-sql sqlalchemy psycopg2 notebook pandas -c conda-forge
```

**Sample dataset:**
- [cities.csv](https://github.com/giswqs/postgis/blob/master/data/cities.csv)

+++

## Using ipython-sql

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

SELECT * from cities LIMIT 10
```

```{code-cell} ipython3
:tags: [remove-output]

%%sql

SELECT * from cities
```

## Using sqlalchemy

```{code-cell} ipython3
from sqlalchemy import create_engine
```

```{code-cell} ipython3
engine = create_engine(connection_string)
```

```{code-cell} ipython3
from sqlalchemy import inspect
```

```{code-cell} ipython3
insp = inspect(engine)
insp.get_table_names()
```

```{code-cell} ipython3
import pandas as pd
```

```{code-cell} ipython3
df = pd.read_sql('SELECT * from cities LIMIT 10', engine)
```

```{code-cell} ipython3
df
```

```{code-cell} ipython3

```
