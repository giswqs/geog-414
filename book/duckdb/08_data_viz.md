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

## Data Visualization

```{code-cell} ipython3
# %pip install -U leafmap lonboard 
```

```{code-cell} ipython3
import leafmap
```

## Visualizing point data

```{code-cell} ipython3
url = 'https://open.gishub.org/data/duckdb/cities.parquet'
```

Read GeoParquet and return a GeoPandas GeoDataFrame.

```{code-cell} ipython3
gdf = leafmap.read_parquet(url, return_type='gdf', src_crs='EPSG:4326')
gdf.head()
```

View the GeoDataFrame interactively using folium.

```{code-cell} ipython3
gdf.explore()
```

Visualize the GeoDataFrame using [lonboard](https://github.com/developmentseed/lonboard).

```{code-cell} ipython3
leafmap.view_vector(gdf, get_radius=20000, get_fill_color='blue')
```

## Visualizing polygon data

```{code-cell} ipython3
url = 'https://data.source.coop/giswqs/nwi/wetlands/DC_Wetlands.parquet'
```

```{code-cell} ipython3
gdf = leafmap.read_parquet(url, return_type='gdf', src_crs='EPSG:5070', dst_crs='EPSG:4326')
gdf.head()
```

```{code-cell} ipython3
gdf.explore()
```

```{code-cell} ipython3
leafmap.view_vector(gdf, get_fill_color=[0, 0, 255, 128])
```

![vector](https://i.imgur.com/HRtpiVd.png)

+++

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
        "Other Freshwater Wetland": (178, 134, 86),
    }
```

```{code-cell} ipython3
leafmap.view_vector(gdf, color_column='WETLAND_TYPE', color_map=color_map, opacity=0.5)
```

![vector-color](https://i.imgur.com/Ejh8hK6.png)

+++

Display a legend for the data.

```{code-cell} ipython3
leafmap.Legend(title="Wetland Type", legend_dict=color_map)
```

![legend](https://i.imgur.com/fxzHHFN.png)

+++

## Visualizing multiple layers

```{code-cell} ipython3
import leafmap.deckgl as leafmap
```

```{code-cell} ipython3
m = leafmap.Map()
countries = 'https://open.gishub.org/data/duckdb/countries.geojson'
cities = 'https://open.gishub.org/data/duckdb/cities.geojson'
m.add_vector(countries, get_fill_color='blue', opacity=0.1)
m.add_vector(cities, get_radius=20000, get_fill_color='red', opacity=0.5)
m
```
