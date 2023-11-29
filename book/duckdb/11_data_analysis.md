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

# Data Analysis

## Introduction

The tutorial uses the [National Wetlands Inventory](https://www.fws.gov/program/national-wetlands-inventory) dataset as an example to demonstrate how to use DuckDB to analyze large geospatial datasets. The National Wetlands Inventory is a [publicly available](https://www.fws.gov/wetlands/Data/Data-Download.html) dataset that provides information on the characteristics, extent, and status of the nation's wetlands and deepwater habitats. The dataset is distributed as zipped Geodatabase files, and is available for download from [here](https://www.fws.gov/program/national-wetlands-inventory/download-state-wetlands-data). I have downloaded the database by state and converted the data to GeoParquet format. The GeoParquet files are available from [Source Cooperative](https://beta.source.coop/repositories/giswqs/nwi/description). The total file size of the 51 Parquet files is 75.8 GB.

## Data download

The script below was used to download the data from the National Wetlands Inventory in Geodatabase format from [here](https://www.fws.gov/program/national-wetlands-inventory/download-state-wetlands-data). The script uses the [leafmap](https://leafmap.org) Python package.

First, create a conda environment with the required packages:

```bash
conda create -n gdal python=3.11
conda activate gdal
conda install -c conda-forge mamba
mamba install -c conda-forge libgdal-arrow-parquet gdal leafmap
pip install lonboard
```

If you are using Google Colab, you can install the packages as follows:

```{code-cell} ipython3
# %pip install leafmap lonboard
```

Then, run the script below:

```{code-cell} ipython3
import leafmap
import pandas as pd

url = 'https://open.gishub.org/data/us/us_states.csv'
df = pd.read_csv(url)
ids = df['id'].tolist()
ids.sort()
urls = [f"https://documentst.ecosphere.fws.gov/wetlands/data/State-Downloads/{id}_geodatabase_wetlands.zip" for id in ids]
leafmap.download_files(urls, out_dir='.', unzip=True)
```

## Data conversion

The script below was used to convert the data from the original Geodatabase format to [Parquet](https://parquet.apache.org) format. The script uses the [leafmap](https://leafmap.org) Python package.

```{code-cell} ipython3
import leafmap
import pandas as pd

url = 'https://open.gishub.org/data/us/us_states.csv'
df = pd.read_csv(url)
ids = df['id'].tolist()

for index, state in enumerate(ids):
    print(f'Processing {state} ({index+1}/{len(ids)})')
    gdb = f"{state}_geodatabase_wetlands.gdb/"
    layer_name = f'{state}_Wetlands'
    leafmap.gdb_to_vector(gdb, ".", gdal_driver="Parquet", layers=[layer_name])
```

## Data access

The script below can be used to access the data using [DuckDB](https://duckdb.org). The script uses the [duckdb](https://duckdb.org) Python package.

```{code-cell} ipython3
import duckdb

con = duckdb.connect()
con.install_extension("spatial")
con.load_extension("spatial")

state = "DC"    # Change to the US State of your choice
url = f"https://data.source.coop/giswqs/nwi/wetlands/{state}_Wetlands.parquet"
con.sql(f"SELECT * EXCLUDE geometry, ST_GeomFromWKB(geometry) FROM '{url}'")
```

Inspect the table schema:

```{code-cell} ipython3
con.sql(f"DESCRIBE FROM '{url}'")
```

Alternatively, you can use the aws cli to access the data directly from the S3 bucket. The can be very useful when you need to access multiple files. 

```bash
aws s3 ls s3://us-west-2.opendata.source.coop/giswqs/nwi/wetlands/
```

## Data visualization

To visualize the data, you can use the [leafmap](https://leafmap.org) Python package with the [lonboard](https://github.com/developmentseed/lonboard) backend. The script below shows how to visualize the data.

```{code-cell} ipython3
import leafmap

state = "DC"   # Change to the US State of your choice
url = f"https://data.source.coop/giswqs/nwi/wetlands/{state}_Wetlands.parquet"
gdf = leafmap.read_parquet(url, return_type='gdf', src_crs='EPSG:5070', dst_crs='EPSG:4326')
leafmap.view_vector(gdf, get_fill_color=[0, 0, 255, 128])
```

![vector](https://i.imgur.com/HRtpiVd.png)

Alternatively, you can specify a color map to visualize the data.

```{code-cell} ipython3
color_map =  {
        "Freshwater Forested/Shrub Wetland": (0, 136, 55),
        "Freshwater Emergent Wetland": (127, 195, 28),
        "Freshwater Pond": (104, 140, 192),
        "Estuarine and Marine Wetland": (102, 194, 165),
        "Riverine": (1, 144, 191),
        "Lake": (19, 0, 124),
        "Estuarine and Marine Deepwater": (0, 124, 136),
        "Other": (178, 134, 86),
    }
leafmap.view_vector(gdf, color_column='WETLAND_TYPE', color_map=color_map, opacity=0.5)
```

![vector-color](https://i.imgur.com/Ejh8hK6.png)

Display a legend for the data.

```{code-cell} ipython3
leafmap.Legend(title="Wetland Type", legend_dict=color_map)
```

![legend](https://i.imgur.com/fxzHHFN.png)

+++

## Data analysis


Find out the total number of wetlands in the United States by aggregating the 51 parquet files.

```{code-cell} ipython3
con.sql(f"""
SELECT COUNT(*) AS Count
FROM 's3://us-west-2.opendata.source.coop/giswqs/nwi/wetlands/*.parquet'
""")
```

Find out the number of wetlands in each state. Note that the NWI datasets do not contain a field for state names. The `filename` argument can be used to add an extra `filename` column to the result that indicates which row came from which file.

```{code-cell} ipython3
df = con.sql(f"""
SELECT filename, COUNT(*) AS Count
FROM read_parquet('s3://us-west-2.opendata.source.coop/giswqs/nwi/wetlands/*.parquet', filename=true)
GROUP BY filename
ORDER BY COUNT(*) DESC;
""").df()
df.head()
```

Inspect the list of filenames.

```{code-cell} ipython3
df['filename'].tolist()
```

Create a `State` column based on the `filename` column by extracting the state name from the filename.

```{code-cell} ipython3
count_df = con.sql(f"""
SELECT SUBSTRING(filename, LENGTH(filename) - 18, 2) AS State, COUNT(*) AS Count
FROM read_parquet('s3://us-west-2.opendata.source.coop/giswqs/nwi/wetlands/*.parquet', filename=true)
GROUP BY State
ORDER BY COUNT(*) DESC;
""").df()
count_df.head(10)
```

Create a `wetlands` table from the DataFrame above.

```{code-cell} ipython3
con.sql("CREATE OR REPLACE TABLE wetlands AS FROM count_df")
con.sql("FROM wetlands")
```

To visualize the data on the map, we need a GeoDataFrame. Let's create a `states` table from the [us_states.parquet](https://open.gishub.org/data/us/us_states.parquet) file.

```{code-cell} ipython3
url = 'https://open.gishub.org/data/us/us_states.parquet'
con.sql(
    f"""
CREATE OR REPLACE TABLE states AS
SELECT * EXCLUDE geometry, ST_GeomFromWKB(geometry) 
AS geometry FROM '{url}'
"""
)
con.sql("FROM states")
```

Join the `wetlands` table with the `states` table. 

```{code-cell} ipython3
con.sql("""
SELECT * FROM states INNER JOIN wetlands ON states.id = wetlands.State
""")
```

Export the joined table as a Pandas DataFrame.

```{code-cell} ipython3
df = con.sql("""
SELECT name, State, Count, ST_AsText(geometry) as geometry
FROM states INNER JOIN wetlands ON states.id = wetlands.State
""").df()
df.head()
```

Convert the Pandas DataFrame to a GeoDataFrame.

```{code-cell} ipython3
gdf = leafmap.df_to_gdf(df, src_crs="EPSG:4326")
```

Visualize the data on the map.

```{code-cell} ipython3
m = leafmap.Map()
m.add_data(
    gdf, column='Count', scheme='Quantiles', cmap='Greens', legend_title='Wetland Count'
)
m
```

![](https://i.imgur.com/x9nJWZR.png)

+++

Create a pie chart to show the percentage of wetlands in each state.

```{code-cell} ipython3
leafmap.pie_chart(count_df, 'State', 'Count', height=800, title='Number of Wetlands by State')
```

![](https://i.imgur.com/EQFZW4x.png)

+++

Create a bar chart to show the number of wetlands in each state.

```{code-cell} ipython3
leafmap.bar_chart(count_df, 'State', 'Count', title='Number of Wetlands by State')
```

![](https://i.imgur.com/dNjh9lp.png)

+++

Calculate the total area of wetlands in the United States. It takes about 3 minutes to run this query. Please be patient.

```{code-cell} ipython3
con.sql(f"""
SELECT SUM(Shape_Area) /  1000000 AS Area_SqKm
FROM 's3://us-west-2.opendata.source.coop/giswqs/nwi/wetlands/*.parquet'
""")
```

Calculate the total area of wetlands in each state. It takes about 3 minutes to run this query. Please be patient.

```{code-cell} ipython3
area_df = con.sql(f"""
SELECT SUBSTRING(filename, LENGTH(filename) - 18, 2) AS State, SUM(Shape_Area) /  1000000 AS Area_SqKm
FROM read_parquet('s3://us-west-2.opendata.source.coop/giswqs/nwi/wetlands/*.parquet', filename=true)
GROUP BY State
ORDER BY COUNT(*) DESC;
""").df()
area_df.head(10)
```

Create a pie chart to show the percentage of wetlands in each state.

```{code-cell} ipython3
leafmap.pie_chart(area_df, 'State', 'Area_SqKm', height=900, title='Wetland Area by State')
```

![](https://i.imgur.com/tIy2fLt.png)

+++

Create a bar chart to show the wetland area in each state.

```{code-cell} ipython3
leafmap.bar_chart(area_df, 'State', 'Area_SqKm', title='Wetland Area by State')
```

![](https://i.imgur.com/EyJQZNP.png)
