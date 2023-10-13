---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.15.0
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Lab 6

**Firstname Lastname**

**Submission instructions**

Submit the Colab link to your notebook in Canvas. In addition, take screenshots of the map for each question and submit them to Canvas as well.

```{code-cell} ipython3
import ee
import geemap
```

```{code-cell} ipython3
geemap.ee_initialize()
```

## Question 1

Create a map to visualize [NOAA GFS Temperature Data](https://developers.google.com/earth-engine/datasets/catalog/NOAA_GFS0P25) and add a color bar and [NOAA logo](https://i.imgur.com/spILFEi.png) to the map. 

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/bBoRswR.png)

+++

## Question 2

**Linked Maps:** Create a 2*2 linked map to visualize the Landsat imagery (`ee.Image('LANDSAT/LE7_TOA_5YEAR/1999_2003')`) with different band combinations.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/Xp15uOo.png)

+++

## Question 3

**Timeseries Inspector:** Create a map with timeseries inspector to visualize [USDA NASS Cropland Data Layers](https://developers.google.com/earth-engine/datasets/catalog/USDA_NASS_CDL) from 2010 to 2022. Add your name and [USDA logo](https://i.imgur.com/tzH2dNr.png) to the map.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/gW1XPhL.png)

+++

## Question 4

**Time slider:** Create a map with the time slider to visualize [Sentinel-2](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR) for Knoxville, TN.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/rJe2rvP.png)

+++

## Question 5

**Split-panel Map:** Use the following datasets to create a split-panel map for visualizing the ESA land cover data in the US. Add the ESA land cover legend to the map (Hint: the built-in legend for ESA land cover is `ESA_WorldCover`).

- [US Census States](https://developers.google.com/earth-engine/datasets/catalog/TIGER_2018_States): `ee.FeatureCollection("TIGER/2018/States")`
- [ESA WorldCover 10m](https://developers.google.com/earth-engine/datasets/catalog/ESA_WorldCover_v100): `ee.ImageCollection("ESA/WorldCover/v100")`
- Landsat: `LANDSAT/LE7_TOA_5YEAR/1999_2003`

+++

Currently, the split-map control of ipyleaflet plotting backend has a bug ([source](https://github.com/jupyter-widgets/ipyleaflet/issues/1066)). Use the folium plotting backend instead.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/y0GW6k8.png)
