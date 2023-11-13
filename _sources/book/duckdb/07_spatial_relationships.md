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

# Spatial Relationships

## Introduction

This notebook demonstrates how to analyze spatial relationships between features in a dataset. 

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

```{code-cell} ipython3
con.sql("SELECT * from nyc_subway_stations LIMIT 5")
```

## Spatial Relationships

So far we have only used spatial functions that measure (`ST_Area`,
`ST_Length`), serialize (`ST_GeomFromText`) or deserialize (`ST_AsGML`)
geometries. What these functions have in common is that they only work
on one geometry at a time.

Spatial databases are powerful because they not only store geometry,
they also have the ability to compare *relationships between
geometries*.

Questions like "Which are the closest bike racks to a park?" or "Where
are the intersections of subway lines and streets?" can only be answered
by comparing geometries representing the bike racks, streets, and subway
lines.

The OGC standard defines the following set of methods to compare
geometries.

+++

## ST_Equals

`ST_Equals(geometry A, geometry B)`tests the spatial equality of two geometries.

![](https://postgis.net/workshops/postgis-intro/_images/st_equals.png)

ST_Equals returns TRUE if two geometries of the same type have identical
x,y coordinate values, i.e. if the second shape is equal (identical) to
the first shape.

First, let\'s retrieve a representation of a point from our
`nyc_subway_stations` table. We\'ll take just the entry for \'Broad
St\'.

```{code-cell} ipython3
con.sql("""
SELECT name, geom, ST_AsText(geom)
FROM nyc_subway_stations
WHERE name = 'Broad St';
""")
```

Then, plug the geometry representation back into an
`ST_Equals` test:

```{code-cell} ipython3
con.sql("""
SELECT name
FROM nyc_subway_stations
WHERE ST_Equals(geom, ST_GeomFromText('POINT (583571.9059213118 4506714.341192182)'));
""")
```

## ST_Intersects, ST_Disjoint, ST_Crosses and ST_Overlaps

`ST_Intersects`,
`ST_Crosses`, and
`ST_Overlaps` test whether the
interiors of the geometries intersect.

![](https://postgis.net/workshops/postgis-intro/_images/st_intersects.png)

`ST_Intersects(geometry A, geometry B)` returns t (TRUE) if the two shapes have any space in
common, i.e., if their boundaries or interiors intersect.

![](https://postgis.net/workshops/postgis-intro/_images/st_disjoint.png)

The opposite of ST_Intersects is
`ST_Disjoint(geometry A , geometry B)`. If two geometries are disjoint, they do not intersect,
and vice-versa. In fact, it is often more efficient to test \"not
intersects\" than to test \"disjoint\" because the intersects tests can
be spatially indexed, while the disjoint test cannot.

![](https://postgis.net/workshops/postgis-intro/_images/st_crosses.png)

For multipoint/polygon, multipoint/linestring, linestring/linestring,
linestring/polygon, and linestring/multipolygon comparisons,
`ST_Crosses(geometry A, geometry B)`
returns t (TRUE) if the intersection results in a geometry whose
dimension is one less than the maximum dimension of the two source
geometries and the intersection set is interior to both source
geometries.

![](https://postgis.net/workshops/postgis-intro/_images/st_overlaps.png)

`ST_Overlaps(geometry A, geometry B)`
compares two geometries of the same dimension and returns TRUE if their
intersection set results in a geometry different from both but of the
same dimension.

Let\'s take our Broad Street subway station and determine its
neighborhood using the `ST_Intersects`
function:

```{code-cell} ipython3
con.sql("""
SELECT name, ST_AsText(geom)
FROM nyc_subway_stations
WHERE name = 'Broad St';
""")
```

```{code-cell} ipython3
con.sql("FROM nyc_neighborhoods LIMIT 5")
```

```{code-cell} ipython3
con.sql("""
SELECT name, boroname
FROM nyc_neighborhoods
WHERE ST_Intersects(geom, ST_GeomFromText('POINT(583571 4506714)'));
""")
```

## ST_Touches

`ST_Touches` tests whether two
geometries touch at their boundaries, but do not intersect in their
interiors

![](https://postgis.net/workshops/postgis-intro/_images/st_touches.png)

`ST_Touches(geometry A, geometry B)`
returns TRUE if either of the geometries\' boundaries intersect or if
only one of the geometry\'s interiors intersects the other\'s boundary.

## ST_Within and ST_Contains

`ST_Within` and
`ST_Contains` test whether one
geometry is fully within the other.

![](https://postgis.net/workshops/postgis-intro/_images/st_within.png)

`ST_Within(geometry A , geometry B)`
returns TRUE if the first geometry is completely within the second
geometry. ST_Within tests for the exact opposite result of ST_Contains.

`ST_Contains(geometry A, geometry B)`
returns TRUE if the second geometry is completely contained by the first
geometry.

## ST_Distance and ST_DWithin

An extremely common GIS question is \"find all the stuff within distance
X of this other stuff\".

The `ST_Distance(geometry A, geometry B)` calculates the *shortest* distance between two
geometries and returns it as a float. This is useful for actually
reporting back the distance between objects.

```{code-cell} ipython3
con.sql("""
SELECT ST_Distance(
  ST_GeomFromText('POINT(0 5)'),
  ST_GeomFromText('LINESTRING(-2 2, 2 2)')) as dist;
""")
```

For testing whether two objects are within a distance of one another,
the `ST_DWithin` function provides an
index-accelerated true/false test. This is useful for questions like
\"how many trees are within a 500 meter buffer of the road?\". You
don\'t have to calculate an actual buffer, you just have to test the
distance relationship.

![](https://postgis.net/workshops/postgis-intro/_images/st_dwithin.png)

Using our Broad Street subway station again, we can find the streets
nearby (within 10 meters of) the subway stop:

```{code-cell} ipython3
con.sql("FROM nyc_streets LIMIT 5")
```

```{code-cell} ipython3
con.sql("""
SELECT name
FROM nyc_streets
WHERE ST_DWithin(
        geom,
        ST_GeomFromText('POINT(583571 4506714)'),
        10
      );
""")
```

And we can verify the answer on a map. The Broad St station is actually
at the intersection of Wall, Broad and Nassau Streets.

![image](https://postgis.net/workshops/postgis-intro/_images/broad_st.jpg)

## Function List

[ST_Contains(geometry A, geometry
B)](http://postgis.net/docs/ST_Contains.html): Returns true if and only
if no points of B lie in the exterior of A, and at least one point of
the interior of B lies in the interior of A.

[ST_Crosses(geometry A, geometry
B)](http://postgis.net/docs/ST_Crosses.html): Returns TRUE if the
supplied geometries have some, but not all, interior points in common.

[ST_Disjoint(geometry A , geometry
B)](http://postgis.net/docs/ST_Disjoint.html): Returns TRUE if the
Geometries do not \"spatially intersect\" - if they do not share any
space together.

[ST_Distance(geometry A, geometry
B)](http://postgis.net/docs/ST_Distance.html): Returns the 2-dimensional
cartesian minimum distance (based on spatial ref) between two geometries
in projected units.

[ST_DWithin(geometry A, geometry B,
radius)](http://postgis.net/docs/ST_DWithin.html): Returns true if the
geometries are within the specified distance (radius) of one another.

[ST_Equals(geometry A, geometry
B)](http://postgis.net/docs/ST_Equals.html): Returns true if the given
geometries represent the same geometry. Directionality is ignored.

[ST_Intersects(geometry A, geometry
B)](http://postgis.net/docs/ST_Intersects.html): Returns TRUE if the
Geometries/Geography \"spatially intersect\" - (share any portion of
space) and FALSE if they don\'t (they are Disjoint).

[ST_Overlaps(geometry A, geometry
B)](http://postgis.net/docs/ST_Overlaps.html): Returns TRUE if the
Geometries share space, are of the same dimension, but are not
completely contained by each other.

[ST_Touches(geometry A, geometry
B)](http://postgis.net/docs/ST_Touches.html): Returns TRUE if the
geometries have at least one point in common, but their interiors do not
intersect.

[ST_Within(geometry A , geometry
B)](http://postgis.net/docs/ST_Within.html): Returns true if the
geometry A is completely inside geometry B
