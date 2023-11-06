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

# Working with Geometries

## Introduction

This notebook demonstrates how to work with geometries in DuckDB.

## Installation

Uncomment the following cell to install the required packages if needed.

```{code-cell} ipython3
# %pip install duckdb leafmap
```

## Library Import and Configuration

```{code-cell} ipython3
import duckdb
import leafmap
```

## Sample Data

The datasets in the database are in NAD83 / UTM zone 18N projection, EPSG:26918.

```{code-cell} ipython3
url = "https://open.gishub.org/data/duckdb/nyc_data.db.zip"
leafmap.download_file(url, unzip=True)
```

## Connecting to DuckDB

Connect jupysql to DuckDB using a SQLAlchemy-style connection string. You may either connect to an in memory DuckDB, or a file backed db.

```{code-cell} ipython3
con = duckdb.connect("nyc_data.db")
```

```{code-cell} ipython3
con.install_extension("spatial")
con.load_extension("spatial")
```

```{code-cell} ipython3
con.sql("SHOW TABLES;")
```

## Creating samples

```{code-cell} ipython3
con.sql("""

CREATE or REPLACE TABLE samples (name VARCHAR, geom GEOMETRY);

INSERT INTO samples VALUES
  ('Point', ST_GeomFromText('POINT(-100 40)')),
  ('Linestring', ST_GeomFromText('LINESTRING(0 0, 1 1, 2 1, 2 2)')),
  ('Polygon', ST_GeomFromText('POLYGON((0 0, 1 0, 1 1, 0 1, 0 0))')),
  ('PolygonWithHole', ST_GeomFromText('POLYGON((0 0, 10 0, 10 10, 0 10, 0 0),(1 1, 1 2, 2 2, 2 1, 1 1))')),
  ('Collection', ST_GeomFromText('GEOMETRYCOLLECTION(POINT(2 0),POLYGON((0 0, 1 0, 1 1, 0 1, 0 0)))'));

SELECT * FROM samples;
          
  """)
```

```{code-cell} ipython3
con.sql("SELECT name, ST_AsText(geom) AS geometry FROM samples;")
```

```{code-cell} ipython3
con.sql("""
        
COPY samples TO 'samples.geojson' (FORMAT GDAL, DRIVER GeoJSON);
        
""")
```

## Points

![](https://postgis.net/workshops/postgis-intro/_images/points.png)

A spatial point represents a single location on the Earth. This point is represented by a single coordinate (including either 2-, 3- or 4-dimensions). Points are used to represent objects when the exact details, such as shape and size, are not important at the target scale. For example, cities on a map of the world can be described as points, while a map of a single state might represent cities as polygons.

```{code-cell} ipython3
con.sql("""

SELECT ST_AsText(geom)
  FROM samples
  WHERE name = 'Point';
        
""")
```

Some of the specific spatial functions for working with points are:

- **ST_X(geom)** returns the X ordinate
- **ST_Y(geom)** returns the Y ordinate

So, we can read the ordinates from a point like this:

```{code-cell} ipython3
con.sql("""

SELECT ST_X(geom), ST_Y(geom)
  FROM samples
  WHERE name = 'Point';
        
""")
```

```{code-cell} ipython3
con.sql("""

SELECT * FROM nyc_subway_stations
        
""")
```

```{code-cell} ipython3
con.sql("""

SELECT name, ST_AsText(geom)
  FROM nyc_subway_stations
  LIMIT 10;
        
""")
```

## Linestrings

![](https://postgis.net/workshops/postgis-intro/_images/lines.png)


A **linestring** is a path between locations. It takes the form of an
ordered series of two or more points. Roads and rivers are typically
represented as linestrings. A linestring is said to be **closed** if it
starts and ends on the same point. It is said to be **simple** if it
does not cross or touch itself (except at its endpoints if it is
closed). A linestring can be both **closed** and **simple**.

The street network for New York (`nyc_streets`) was loaded earlier in
the workshop. This dataset contains details such as name, and type. A
single real world street may consist of many linestrings, each
representing a segment of road with different attributes.

The following SQL query will return the geometry associated with one
linestring (in the `ST_AsText` column).

```{code-cell} ipython3
con.sql("""
        
SELECT ST_AsText(geom)
  FROM samples
  WHERE name = 'Linestring';
        
""")
```

Some of the specific spatial functions for working with linestrings are:

-   `ST_Length(geom)` returns the length of the linestring
-   `ST_StartPoint(geom)` returns the first coordinate as a point
-   `ST_EndPoint(geom)` returns the last coordinate as a point
-   `ST_NPoints(geom)` returns the number of coordinates in the
    linestring

So, the length of our linestring is:

```{code-cell} ipython3
con.sql("""

SELECT ST_Length(geom)
  FROM samples
  WHERE name = 'Linestring';
        
""")
```

## Polygons

+++

![](https://postgis.net/workshops/postgis-intro/_images/polygons.png)

A polygon is a representation of an area. The outer boundary of the
polygon is represented by a ring. This ring is a linestring that is both
closed and simple as defined above. Holes within the polygon are also
represented by rings.

Polygons are used to represent objects whose size and shape are
important. City limits, parks, building footprints or bodies of water
are all commonly represented as polygons when the scale is sufficiently
high to see their area. Roads and rivers can sometimes be represented as
polygons.

The following SQL query will return the geometry associated with one
polygon (in the `ST_AsText` column).

```{code-cell} ipython3
con.sql("""

SELECT ST_AsText(geom)
  FROM samples
  WHERE name LIKE 'Polygon%';
        
""")
```

Some of the specific spatial functions for working with polygons are:

-   `ST_Area(geom)` returns the area of the polygons
-   `ST_NRings(geom)` returns the number of rings (usually 1, more
    of there are holes)
-   `ST_ExteriorRing(geom)` returns the outer ring as a linestring
-   `ST_InteriorRingN(geometry,n)` returns a specified interior ring as
    a linestring
-   `ST_Perimeter(geom)` returns the length of all the rings

We can calculate the area of our polygons using the area function:

```{code-cell} ipython3
con.sql("""
        
SELECT name, ST_Area(geom)
  FROM samples
  WHERE name LIKE 'Polygon%';
        
""")
```

## Collections

There are four collection types, which group multiple simple samples
into sets.

-   **MultiPoint**, a collection of points
-   **MultiLineString**, a collection of linestrings
-   **MultiPolygon**, a collection of polygons
-   **GeometryCollection**, a heterogeneous collection of any geometry
    (including other collections)

Collections are another concept that shows up in GIS software more than
in generic graphics software. They are useful for directly modeling real
world objects as spatial objects. For example, how to model a lot that
is split by a right-of-way? As a **MultiPolygon**, with a part on either
side of the right-of-way.

Our example collection contains a polygon and a point:

```{code-cell} ipython3
con.sql("""

SELECT name, ST_AsText(geom)
  FROM samples
  WHERE name = 'Collection';
        
""")
```

## Data Visualization

```{code-cell} ipython3
con.sql("SHOW TABLES;")
```

```{code-cell} ipython3
subway_stations_df = con.sql("SELECT * EXCLUDE geom, ST_AsText(geom) as geometry FROM nyc_subway_stations").df()
subway_stations_df.head()
```

```{code-cell} ipython3
subway_stations_gdf = leafmap.df_to_gdf(subway_stations_df, src_crs="EPSG:26918", dst_crs="EPSG:4326")
subway_stations_gdf.head()
```

```{code-cell} ipython3
subway_stations_gdf.explore()
```

```{code-cell} ipython3
nyc_streets_df = con.sql("SELECT * EXCLUDE geom, ST_AsText(geom) as geometry FROM nyc_streets").df()
nyc_streets_df.head()
```

```{code-cell} ipython3
nyc_streets_gdf = leafmap.df_to_gdf(nyc_streets_df, src_crs="EPSG:26918", dst_crs="EPSG:4326")
nyc_streets_gdf.head()
```

```{code-cell} ipython3
nyc_streets_gdf.explore()
```
