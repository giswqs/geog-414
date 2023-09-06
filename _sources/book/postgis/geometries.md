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

# Working with Geometries

**Setting up the conda env:**

```
conda create -n sql python
conda activate sql
conda install ipython-sql sqlalchemy psycopg2 notebook pandas -c conda-forge
```

**Sample dataset:**
- [nyc_data.zip](https://github.com/giswqs/postgis/raw/master/data/nyc_data.zip) (Watch this [video](https://youtu.be/fROzLrjNDrs) to load data into PostGIS)

**References**:
- [Introduction to PostGIS](https://postgis.net/workshops/postgis-intro)
- [Using SQL with Geodatabases](https://desktop.arcgis.com/en/arcmap/latest/manage-data/using-sql-with-gdbs/sql-and-enterprise-geodatabases.htm)

+++

## Connecting to the database

```{code-cell} ipython3
%load_ext sql
```

```{code-cell} ipython3
import os
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
%sql $connection_string
```

```{code-cell} ipython3
%%sql 

SELECT * FROM nyc_neighborhoods WHERE FALSE
```

## Creating geometries

```{code-cell} ipython3
%%sql

CREATE TABLE geometries (name varchar, geom geometry);

INSERT INTO geometries VALUES
  ('Point', 'POINT(0 0)'),
  ('Linestring', 'LINESTRING(0 0, 1 1, 2 1, 2 2)'),
  ('Polygon', 'POLYGON((0 0, 1 0, 1 1, 0 1, 0 0))'),
  ('PolygonWithHole', 'POLYGON((0 0, 10 0, 10 10, 0 10, 0 0),(1 1, 1 2, 2 2, 2 1, 1 1))'),
  ('Collection', 'GEOMETRYCOLLECTION(POINT(2 0),POLYGON((0 0, 1 0, 1 1, 0 1, 0 0)))');

SELECT name, ST_AsText(geom) FROM geometries;
```

## Metadata tables

```{code-cell} ipython3
%%sql 

SELECT * FROM spatial_ref_sys LIMIT 10
```

```{code-cell} ipython3
%%sql

SELECT * FROM geometry_columns
```

```{code-cell} ipython3
%%sql 

SELECT name, ST_GeometryType(geom), ST_NDims(geom), ST_SRID(geom)
  FROM geometries;
```

## Points

![](https://postgis.net/workshops/postgis-intro/_images/points.png)

A spatial point represents a single location on the Earth. This point is represented by a single coordinate (including either 2-, 3- or 4-dimensions). Points are used to represent objects when the exact details, such as shape and size, are not important at the target scale. For example, cities on a map of the world can be described as points, while a map of a single state might represent cities as polygons.

```{code-cell} ipython3
%%sql

SELECT ST_AsText(geom)
  FROM geometries
  WHERE name = 'Point';
```

Some of the specific spatial functions for working with points are:

- **ST_X(geometry)** returns the X ordinate
- **ST_Y(geometry)** returns the Y ordinate

So, we can read the ordinates from a point like this:

```{code-cell} ipython3
%%sql

SELECT ST_X(geom), ST_Y(geom)
  FROM geometries
  WHERE name = 'Point';
```

```{code-cell} ipython3
%%sql

SELECT name, ST_AsText(geom)
  FROM nyc_subway_stations
  LIMIT 10;
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
%%sql

SELECT ST_AsText(geom)
  FROM geometries
  WHERE name = 'Linestring';
```

Some of the specific spatial functions for working with linestrings are:

-   `ST_Length(geometry)` returns the length of the linestring
-   `ST_StartPoint(geometry)` returns the first coordinate as a point
-   `ST_EndPoint(geometry)` returns the last coordinate as a point
-   `ST_NPoints(geometry)` returns the number of coordinates in the
    linestring

So, the length of our linestring is:

```{code-cell} ipython3
%%sql 

SELECT ST_Length(geom)
  FROM geometries
  WHERE name = 'Linestring';
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
%%sql

SELECT ST_AsText(geom)
  FROM geometries
  WHERE name LIKE 'Polygon%';
```

Some of the specific spatial functions for working with polygons are:

-   `ST_Area(geometry)` returns the area of the polygons
-   `ST_NRings(geometry)` returns the number of rings (usually 1, more
    of there are holes)
-   `ST_ExteriorRing(geometry)` returns the outer ring as a linestring
-   `ST_InteriorRingN(geometry,n)` returns a specified interior ring as
    a linestring
-   `ST_Perimeter(geometry)` returns the length of all the rings

We can calculate the area of our polygons using the area function:

```{code-cell} ipython3
%%sql

SELECT name, ST_Area(geom)
  FROM geometries
  WHERE name LIKE 'Polygon%';
```

## Collections

There are four collection types, which group multiple simple geometries
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
%%sql

SELECT name, ST_AsText(geom)
  FROM geometries
  WHERE name = 'Collection';
```

Some of the specific spatial functions for working with collections are:

-   `ST_NumGeometries(geometry)` returns the number of parts in the
    collection
-   `ST_GeometryN(geometry,n)` returns the specified part
-   `ST_Area(geometry)` returns the total area of all polygonal parts
-   `ST_Length(geometry)` returns the total length of all linear parts

+++

## Geometry Input and Output

Within the database, geometries are stored on disk in a format only used
by the PostGIS program. In order for external programs to insert and
retrieve useful geometries, they need to be converted into a format that
other applications can understand. Fortunately, PostGIS supports
emitting and consuming geometries in a large number of formats:

-   Well-known text ([`WKT`](https://postgis.net/workshops/postgis-intro/glossary.html#term-wkt))
    -   `ST_GeomFromText(text, srid)` returns `geometry`
    -   `ST_AsText(geometry)` returns `text`
    -   `ST_AsEWKT(geometry)` returns `text`
-   Well-known binary (`WKB`)
    -   `ST_GeomFromWKB(bytea)` returns `geometry`
    -   `ST_AsBinary(geometry)` returns `bytea`
    -   `ST_AsEWKB(geometry)` returns `bytea`
-   Geographic Mark-up Language (`GML`)
    -   `ST_GeomFromGML(text)` returns `geometry`
    -   `ST_AsGML(geometry)` returns `text`
-   Keyhole Mark-up Language (`KML`)
    -   `ST_GeomFromKML(text)` returns `geometry`
    -   `ST_AsKML(geometry)` returns `text`
-   `GeoJSON`
    -   `ST_AsGeoJSON(geometry)` returns `text`
-   Scalable Vector Graphics (`SVG`)
    -   `ST_AsSVG(geometry)` returns `text`

In addition to the `ST_GeometryFromText` function, there are many other
ways to create geometries from well-known text or similar formatted
inputs:

```{code-cell} ipython3
%%sql

-- Using ST_GeomFromText with the SRID parameter
SELECT ST_GeomFromText('POINT(2 2)',4326);

-- Using ST_GeomFromText without the SRID parameter
SELECT ST_SetSRID(ST_GeomFromText('POINT(2 2)'),4326);

-- Using a ST_Make* function
SELECT ST_SetSRID(ST_MakePoint(2, 2), 4326);

-- Using PostgreSQL casting syntax and ISO WKT
SELECT ST_SetSRID('POINT(2 2)'::geometry, 4326);

-- Using PostgreSQL casting syntax and extended WKT
SELECT 'SRID=4326;POINT(2 2)'::geometry;
```

## Casting from Text

The `WKT` strings we've see so far have been of type 'text' and we have
been converting them to type 'geometry' using PostGIS functions like
`ST_GeomFromText()`.

PostgreSQL includes a short form syntax that allows data to be converted
from one type to another, the casting syntax, <span
class="title-ref">oldata::newtype</span>. So for example, this SQL
converts a double into a text string.

```{code-cell} ipython3
%%sql

SELECT 0.9::text;
```

Less trivially, this SQL converts a WKT string into a geometry:

```{code-cell} ipython3
%%sql

SELECT 'POINT(0 0)'::geometry;
```

One thing to note about using casting to create geometries: unless you specify the SRID, you will get a geometry with an unknown SRID. You can specify the SRID using the “extended” well-known text form, which includes an SRID block at the front:

```{code-cell} ipython3
%%sql

SELECT 'SRID=4326;POINT(0 0)'::geometry;
```

## Function List


[ST\_Area][]: Returns the area of the surface if it is a polygon or
multi-polygon. For "geometry" type area is in SRID units. For
"geography" area is in square meters.

[ST\_AsText][]: Returns the Well-Known Text (WKT) representation of the
geometry/geography without SRID metadata.

[ST\_AsBinary][]: Returns the Well-Known Binary (WKB) representation of
the geometry/geography without SRID meta data.

[ST\_EndPoint][]: Returns the last point of a LINESTRING geometry as a
POINT.

[ST\_AsEWKB][]: Returns the Well-Known Binary (WKB) representation of
the geometry with SRID meta data.

[ST\_AsEWKT][]: Returns the Well-Known Text (WKT) representation of the
geometry with SRID meta data.

[ST\_AsGeoJSON][]: Returns the geometry as a GeoJSON element.

[ST\_AsGML][]: Returns the geometry as a GML version 2 or 3 element.

[ST\_AsKML][]: Returns the geometry as a KML element. Several variants.
Default version=2, default precision=15.

[ST\_AsSVG][]: Returns a Geometry in SVG path data given a geometry or
geography object.

[ST\_ExteriorRing][]: Returns a line string representing the exterior
ring of the POLYGON geometry. Return NULL if the geometry is not a
polygon. Will not work with MULTIPOLYGON

[ST\_GeometryN][]: Returns the 1-based Nth geometry if the geometry is a
GEOMETRYCOLLECTION, MULTIPOINT, MULTILINESTRING, MULTICURVE or
MULTIPOLYGON. Otherwise, return NULL.

[ST\_GeomFromGML][]: Takes as input GML representation of geometry and
outputs a PostGIS geometry object.

[ST\_GeomFromKML][]: Takes as input KML representation of geometry and
outputs a PostGIS geometry object

[ST\_GeomFromText][]: Returns a specified ST\_Geometry value from
Well-Known Text representation (WKT).

[ST\_GeomFromWKB][]: Creates a geometry instance from a Well-Known
Binary geometry representation (WKB) and optional SRID.

[ST\_GeometryType][]: Returns the geometry type of the ST\_Geometry
value.

[ST\_InteriorRingN][]: Returns the Nth interior linestring ring of the
polygon geometry. Return NULL if the geometry is not a polygon or the
given N is out of range.

[ST\_Length][]: Returns the 2d length of the geometry if it is a
linestring or multilinestring. geometry are in units of spatial
reference and geogra

  [ST\_Area]: http://postgis.net/docs/ST_Area.html
  [ST\_AsText]: http://postgis.net/docs/ST_AsText.html
  [ST\_AsBinary]: http://postgis.net/docs/ST_AsBinary.html
  [ST\_EndPoint]: http://postgis.net/docs/ST_EndPoint.html
  [ST\_AsEWKB]: http://postgis.net/docs/ST_AsEWKB.html
  [ST\_AsEWKT]: http://postgis.net/docs/ST_AsEWKT.html
  [ST\_AsGeoJSON]: http://postgis.net/docs/ST_AsGeoJSON.html
  [ST\_AsGML]: http://postgis.net/docs/ST_AsGML.html
  [ST\_AsKML]: http://postgis.net/docs/ST_AsKML.html
  [ST\_AsSVG]: http://postgis.net/docs/ST_AsSVG.html
  [ST\_ExteriorRing]: http://postgis.net/docs/ST_ExteriorRing.html
  [ST\_GeometryN]: http://postgis.net/docs/ST_GeometryN.html
  [ST\_GeomFromGML]: http://postgis.net/docs/ST_GeomFromGML.html
  [ST\_GeomFromKML]: http://postgis.net/docs/ST_GeomFromKML.html
  [ST\_GeomFromText]: http://postgis.net/docs/ST_GeomFromText.html
  [ST\_GeomFromWKB]: http://postgis.net/docs/ST_GeomFromWKB.html
  [ST\_GeometryType]: http://postgis.net/docs/ST_GeometryType.html
  [ST\_InteriorRingN]: http://postgis.net/docs/ST_InteriorRingN.html
  [ST\_Length]: http://postgis.net/docs/ST_Length.html
