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

# Lab 7

**Submission instructions**

Submit the Colab link to your notebook in Canvas. In addition, take screenshots of the map for each question and submit them to Canvas as well.

```{code-cell} ipython3
import ee
import geemap
```

```{code-cell} ipython3
geemap.ee_initialize()
```

## Datasets

The datasets being used in the lab are listed below:

- [TIGER: US Census Counties](https://developers.google.com/earth-engine/datasets/catalog/TIGER_2018_Counties)
- [Landsat-9](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC09_C02_T1_L2)
- [Sentinel-2](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR)
- [NAIP](https://developers.google.com/earth-engine/datasets/catalog/USDA_NAIP_DOQQ)

+++

## Question 1

Create a fishnet with a 4-degree interval based on the extent of `[-112.5439, 34.0891, -85.0342, 49.6858]`. Use the fishnet to download the Landsat 7 image tiles by the fishnet using the `geemap.download_ee_image_tiles()` function. Relevant Earth Engine assets:

-   `ee.Image('LANDSAT/LE7_TOA_5YEAR/1999_2003')`

![](https://i.imgur.com/L1IH3fq.png)

```{code-cell} ipython3
# Add your code here.
```

## Question 2

Create annual cloud-free Landsat imagery for the years 2017-2023 for a US county of your choice. Download the images to your computer. 

![](https://i.imgur.com/MN2UXHx.png)

```{code-cell} ipython3
# Add your code here.
```

## Question 3

Create annual cloud-free Sentinel-2 imagery for the years 2017-2023 for a US county of your choice. Download the images to your computer. You can download a coarse resolution image to speed up the download process. Narrow down the date range (e.g., summer months) to reduce the number of images, which can avoid memory errors.

![](https://i.imgur.com/r5RQlEJ.png)

```{code-cell} ipython3
# Add your code here.
```

## Question 4

Create annual cloud-free NAIP imagery for the years 2010-2023 for a US county of your choice. Download the images to your computer. You can download a coarse resolution image to speed up the download process. 

![](https://i.imgur.com/h66FC8h.png)

```{code-cell} ipython3
# Add your code here.
```

## Question 5

Download a US county of your choice and save it as a shapefile or GeoJSON file. 

![](https://i.imgur.com/PuK2Vp3.png)

```{code-cell} ipython3
# Add your code here.
```
