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

# Using GeoPandas

**Setting up the conda env:**

```
conda create -n geo python=3.8
conda activate geo
conda install mamba -c conda-forge
mamba install geemap geopandas descartes rtree=0.9.3 -c conda-forge
mamba install ipython-sql sqlalchemy psycopg2 -c conda-forge
```

**Sample dataset:**

- [nyc_data.zip](https://github.com/giswqs/postgis/raw/master/data/nyc_data.zip) (Watch this [video](https://youtu.be/fROzLrjNDrs) to load data into PostGIS)

**References**:

- [Introduction to PostGIS](https://postgis.net/workshops/postgis-intro)
- [Using SQL with Geodatabases](https://desktop.arcgis.com/en/arcmap/latest/manage-data/using-sql-with-gdbs/sql-and-enterprise-geodatabases.htm)

+++

## Connecting to the database

```{code-cell} ipython3
import os
from sqlalchemy import create_engine
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
engine = create_engine(connection_string)
```

```{code-cell} ipython3
from sqlalchemy import inspect
```

```{code-cell} ipython3
insp = inspect(engine)
insp.get_table_names()
```

## Reading data from PostGIS

```{code-cell} ipython3
import geopandas as gpd
```

```{code-cell} ipython3
sql = 'SELECT * FROM nyc_neighborhoods'
```

```{code-cell} ipython3
gdf = gpd.read_postgis(sql, con=engine)
```

```{code-cell} ipython3
gdf
```

```{code-cell} ipython3
gdf.crs
```

## Writing files

```{code-cell} ipython3
out_dir = os.path.expanduser('~/Downloads')
if not os.path.exists(out_dir):
    os.makedirs(out_dir)
```

```{code-cell} ipython3
out_json = os.path.join(out_dir, 'nyc_neighborhoods.geojson')
gdf.to_file(out_json, driver="GeoJSON")
```

```{code-cell} ipython3
out_shp = os.path.join(out_dir, 'nyc_neighborhoods.shp')
gdf.to_file(out_shp)
```

```{code-cell} ipython3
gdf.crs
```

## Measuring area

```{code-cell} ipython3
gdf = gdf.set_index("name")
```

```{code-cell} ipython3
gdf["area"] = gdf.area
gdf["area"]
```

## Getting polygon boundary

```{code-cell} ipython3
gdf['boundary'] = gdf.boundary
gdf['boundary']
```

## Getting polygon centroid

```{code-cell} ipython3
gdf['centroid'] = gdf.centroid
gdf['centroid']
```

## Making maps

```{code-cell} ipython3
gdf.plot()
```

```{code-cell} ipython3
gdf.plot("area", legend=True, figsize=(10, 8))
```

```{code-cell} ipython3
gdf = gdf.set_geometry("centroid")
gdf.plot("area", legend=True,figsize=(10, 8))
```

```{code-cell} ipython3
ax = gdf["geom"].plot(figsize=(10, 8))
gdf["centroid"].plot(ax=ax, color="black")
```

```{code-cell} ipython3
gdf = gdf.set_geometry("geom")
```

## Reprojecting data

```{code-cell} ipython3
sql = 'SELECT * FROM nyc_neighborhoods'
```

```{code-cell} ipython3
gdf = gpd.read_postgis(sql, con=engine)
```

```{code-cell} ipython3
gdf_crs = gdf.to_crs(epsg="4326")
```

```{code-cell} ipython3
gdf_crs
```

```{code-cell} ipython3
geojson = gdf_crs.__geo_interface__
```

## Displaying data on an interactive map

```{code-cell} ipython3
import geemap
```

```{code-cell} ipython3
m = geemap.Map(center=[40.7341, -73.9113], zoom=10, ee_initialize=False)
m
```

```{code-cell} ipython3
style = {
    "stroke": True,
    "color": "#000000",
    "weight": 2,
    "opacity": 1,
    "fill": True,
    "fillColor": "#0000ff",
    "fillOpacity": 0.4,
}
```

```{code-cell} ipython3
m.add_geojson(geojson, style=style, layer_name="nyc neighborhoods")
```

```{code-cell} ipython3
sql2 = 'SELECT * FROM nyc_subway_stations'
```

```{code-cell} ipython3
gdf_subway = gpd.read_postgis(sql2, con=engine)
```

```{code-cell} ipython3
gdf_subway_crs = gdf_subway.to_crs(epsg="4326")
```

```{code-cell} ipython3
subway_geojson = gdf_subway_crs.__geo_interface__
```

```{code-cell} ipython3
m.add_geojson(subway_geojson, layer_name="nyc subway stations")
```

```{code-cell} ipython3
sql3 = "SELECT * FROM nyc_census_blocks WHERE boroname='Manhattan'"
```

```{code-cell} ipython3
gdf_blocks = gpd.read_postgis(sql3, con=engine)
```

```{code-cell} ipython3
gdf_blocks_crs = gdf_blocks.to_crs(epsg="4326")
```

```{code-cell} ipython3
blocks_geojson = gdf_blocks_crs.__geo_interface__
```

```{code-cell} ipython3
m.add_geojson(blocks_geojson, style=style, layer_name="nyc census blocks")
```

```{code-cell} ipython3
sql4 = "SELECT geom FROM nyc_homicides WHERE boroname='Manhattan'"
```

```{code-cell} ipython3
gdf_homicides = gpd.read_postgis(sql4, con=engine)
```

```{code-cell} ipython3
gdf_homicides_crs = gdf_homicides.to_crs(epsg="4326")
```

```{code-cell} ipython3
homicides_geojson =gdf_homicides_crs.__geo_interface__
```

```{code-cell} ipython3
m.add_geojson(homicides_geojson, style=style, layer_name="nyc homicides")
```

```{code-cell} ipython3

```
