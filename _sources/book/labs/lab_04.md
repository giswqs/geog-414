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

# Lab 4

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

## Datasets

The datasets being used in the lab are listed below:

- [TIGER: US Census Counties](https://developers.google.com/earth-engine/datasets/catalog/TIGER_2018_Counties)
- [Landsat-9](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC09_C02_T1_L2)
- [Sentinel-2](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR)
- [NAIP](https://developers.google.com/earth-engine/datasets/catalog/USDA_NAIP_DOQQ)

+++

## Question 1

Write a program to find out how many counties are named `Knox` in the US.

```{code-cell} ipython3
# Add your code here
```

![](https://i.imgur.com/3Jg9P6X.png)

+++

## Question 2

Display Knox county of Tennesse with outline only (no fill color) on the map. (Hint: The `STATEFP` of Tennessee is `47`)

```{code-cell} ipython3
# Add your code here
```

![](https://i.imgur.com/28Iaw9b.png)

+++

## Question 3

Use [Landsat-9](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC09_C02_T1_L2) data to create a cloud-free imagery for Knox County, TN. Display the imagery on the map with a proper band combination.

```{code-cell} ipython3
# Add your code here
```

![](https://i.imgur.com/m72amN0.png)

+++

## Question 4

Use [Sentinel-2](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR) data to create a cloud-free imagery for Knox County, TN. Display the imagery on the map with a proper band combination.

```{code-cell} ipython3
# Add your code here
```

![](https://i.imgur.com/vM1M8Gc.png)

+++

## Question 5

Use [NAIP](https://developers.google.com/earth-engine/datasets/catalog/USDA_NAIP_DOQQ) imagery to create a cloud-free imagery for Knox County, TN. Display the imagery on the map with a proper band combination.

```{code-cell} ipython3
# Add your code here
```

![](https://i.imgur.com/iZSGqGS.png)
