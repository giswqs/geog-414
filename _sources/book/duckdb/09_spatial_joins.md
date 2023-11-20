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

# Spatial Joins

## Introduction

Spatial joins are the bread-and-butter of spatial databases. They allow you to combine information from different tables by using spatial relationships as the join key. Much of what we think of as “standard GIS analysis” can be expressed as spatial joins.

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
con = duckdb.connect('nyc_data.db')
```

```{code-cell} ipython3
con.install_extension('spatial')
con.load_extension('spatial')
```

```{code-cell} ipython3
con.sql("SHOW TABLES;")
```

## Intersection

In the previous section, we explored spatial relationships using a two-step process: first we extracted a subway station point for ‘Broad St’; then, we used that point to ask further questions such as “what neighborhood is the ‘Broad St’ station in?”

Using a spatial join, we can answer the question in one step, retrieving information about the subway station and the neighborhood that contains it. 

Let's start by looking at the subway stations and neighborhoods separately.

```{code-cell} ipython3
con.sql("FROM nyc_neighborhoods SELECT * LIMIT 5;")
```

```{code-cell} ipython3
con.sql("FROM nyc_subway_stations SELECT * LIMIT 5;")
```

Let's find out what neighborhood the `Broad St` station is in:

```{code-cell} ipython3
con.sql("""
SELECT
  subways.name AS subway_name,
  neighborhoods.name AS neighborhood_name,
  neighborhoods.boroname AS borough
FROM nyc_neighborhoods AS neighborhoods
JOIN nyc_subway_stations AS subways
ON ST_Intersects(neighborhoods.geom, subways.geom)
WHERE subways.NAME = 'Broad St';
""")
```

Note that the subway stations table has a `color` column. 

```{code-cell} ipython3
con.sql("""
SELECT DISTINCT COLOR FROM nyc_subway_stations;
""")
```

Let's find out what neighborhood the `RED` subway stations are in:

```{code-cell} ipython3
con.sql("""
SELECT
  subways.name AS subway_name,
  neighborhoods.name AS neighborhood_name,
  neighborhoods.boroname AS borough
FROM nyc_neighborhoods AS neighborhoods
JOIN nyc_subway_stations AS subways
ON ST_Intersects(neighborhoods.geom, subways.geom)
WHERE subways.color = 'RED';
""")
```

## Distance Within

One of the common spatial operations is to find all the features within a certain distance of another feature. For example, you might want to find all the subway stations within 500 meters of a bike share station. Let’s explore the racial geography of New York using distance queries.

First, let’s get the baseline racial make-up of the city.

```{code-cell} ipython3
con.sql("""
SELECT
  100.0 * Sum(popn_white) / Sum(popn_total) AS white_pct,
  100.0 * Sum(popn_black) / Sum(popn_total) AS black_pct,
  Sum(popn_total) AS popn_total
FROM nyc_census_blocks;
""")
```

So, of the 8M people in New York, about 44% are recorded as “white” and 26% are recorded as “black”.

Note that the contents of the `nyc_subway_stations` table routes field is what we are interested in to find the A-train. The values in there are a little complex.

```{code-cell} ipython3
con.sql("""
SELECT DISTINCT routes FROM nyc_subway_stations;
""")
```

So to find the A-train, we will want any row in `routes` that has an ‘A’ in it. We can do this a number of ways, but here we will use the fact that **strpos(routes,'A')** will return a non-zero number only if ‘A’ is in the `routes` field.

```{code-cell} ipython3
con.sql("""
SELECT DISTINCT routes
FROM nyc_subway_stations AS subways
WHERE strpos(subways.routes,'A') > 0;
""")
```

Let’s summarize the racial make-up of within 200 meters of the A-train line.

```{code-cell} ipython3
con.sql("""
SELECT
  100.0 * Sum(popn_white) / Sum(popn_total) AS white_pct,
  100.0 * Sum(popn_black) / Sum(popn_total) AS black_pct,
  Sum(popn_total) AS popn_total
FROM nyc_census_blocks AS census
JOIN nyc_subway_stations AS subways
ON ST_DWithin(census.geom, subways.geom, 200)
WHERE strpos(subways.routes,'A') > 0;
""")
```

So the racial make-up along the A-train isn’t radically different from the make-up of New York City as a whole.

## Advanced Join

In the last section we saw that the A-train didn’t serve a population that differed much from the racial make-up of the rest of the city. Are there any trains that have a non-average racial make-up?

To answer that question, we’ll add another join to our query, so that we can simultaneously calculate the make-up of many subway lines at once. To do that, we’ll need to create a new table that enumerates all the lines we want to summarize.

```{code-cell} ipython3
con.sql("""
CREATE OR REPLACE TABLE subway_lines ( route char(1) );
INSERT INTO subway_lines (route) VALUES
  ('A'),('B'),('C'),('D'),('E'),('F'),('G'),
  ('J'),('L'),('M'),('N'),('Q'),('R'),('S'),
  ('Z'),('1'),('2'),('3'),('4'),('5'),('6'),
  ('7');
""")
```

Now we can join the table of subway lines onto our original query.

```{code-cell} ipython3
con.sql("""
SELECT
  lines.route,
  100.0 * Sum(popn_white) / Sum(popn_total) AS white_pct,
  100.0 * Sum(popn_black) / Sum(popn_total) AS black_pct,
  Sum(popn_total) AS popn_total
FROM nyc_census_blocks AS census
JOIN nyc_subway_stations AS subways
ON ST_DWithin(census.geom, subways.geom, 200)
JOIN subway_lines AS lines
ON strpos(subways.routes, lines.route) > 0
GROUP BY lines.route
ORDER BY black_pct DESC;
""")
```

As before, the joins create a virtual table of all the possible combinations available within the constraints of the JOIN ON restrictions, and those rows are then fed into a GROUP summary. The spatial magic is in the ST_DWithin function, that ensures only census blocks close to the appropriate subway stations are included in the calculation.

+++

## Projection

+++

DuckDB provides the `ST_Transform` function to transform geometries from one projection to another. The function takes three arguments: the geometry to transform, and the EPSG code of the projection of the input geometry, and the EPSG code of the projection to transform to.

```{code-cell} ipython3
url = 'https://open.gishub.org/data/duckdb/cities.parquet'
con.sql(f"SELECT * EXCLUDE geometry, ST_GeomFromWKB(geometry) AS geometry FROM '{url}'")
```

Let's convert the data from EPSG:4326 to [EPSG:5070](https://epsg.io/5070-1252) (NAD83 / Conus Albers).

```{code-cell} ipython3
con.sql(f"""
SELECT * EXCLUDE geometry, ST_Transform(ST_GeomFromWKB(geometry), 'EPSG:4326', 'EPSG:5070', true) AS geometry FROM '{url}'
""")
```

## Function List

https://duckdb.org/docs/archive/0.9.2/extensions/spatial#spatial-relationships

![](https://i.imgur.com/ogJojVX.png)

+++

## References

- [Introduction to PostGIS - Spatial Joins](https://postgis.net/workshops/postgis-intro/joins.html)
