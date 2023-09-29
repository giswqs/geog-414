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

# Lab 5

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

Visualize the [USGS Watershed Boundary Dataset](https://developers.google.com/earth-engine/datasets/catalog/USGS_WBD_2017_HUC04) with outline color only, no fill color.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/PLlNFq3.png)

+++

## Question 2 

Filter the USGS Watershed Boundary dataset and select the watershed that intersects the county of your choice.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/F2QfqZu.png)

+++

## Question 3

Clip the [USGS 3DEP 10m DEM](https://developers.google.com/earth-engine/datasets/catalog/USGS_3DEP_10m) with the watershed that intersects the county of your choice. Display the DEM with a proper color palette and color bar.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/okR39pf.png)

+++

## Question 4

Use the [USGS National Land Cover Database](https://developers.google.com/earth-engine/datasets/catalog/USGS_NLCD_RELEASES_2019_REL_NLCD) and [US Census States](https://developers.google.com/earth-engine/datasets/catalog/TIGER_2018_States) to create a split-panel map for visualizing land cover change (2001-2019) for a US state of your choice. Make sure you add the NLCD legend to the map.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/Au7Q5Ln.png)

+++

## Questions 5

Download OpenStreetMap data for a city of your choice and visualize the city boundary and restaurants in the city.

```{code-cell} ipython3
# Add your code here.
```

![](https://i.imgur.com/AUlO1CV.png)
