---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.15.2
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# Visualizing PMTiles

[PMTiles](https://github.com/protomaps/PMTiles) is a single-file archive format for tiled data. A PMTiles archive can be hosted on a common storage platform such as S3, and enables low-cost, zero-maintenance map applications that are "serverless" - free of a custom tile backend or third party provider.

+++

## Installation

Uncomment and run the following cell to install the dependencies.

```{code-cell} ipython3
# %pip install -U "leafmap[pmtiles]"
```

## Import libraries

Currently, ipyleaflet does not support PMTiles. We will use folium mapping backend with leafmap.

```{code-cell} ipython3
import leafmap.foliumap as leafmap
```

## PMTiles Viewer

The [PMTiles Viewer](https://protomaps.github.io/PMTiles) can be used to view the contents of a PMTiles archive using a web browser. This is a useful tool for visualizing the contents of a PMTiles archive without writing any code. However, you can't use it with Jupyter notebook.

+++

## Remote PMTiles

PMTiles can be hosted on a cloud storage platform or locally. In this section, we will visualize a PMTiles hosted on a remote server.

### Protomaps sample data

The [PMTiles Viewer](https://protomaps.github.io/PMTiles) provides a list of sample PMTiles archives. We will use the [ODbL_firenze.pmtiles](<https://protomaps.github.io/PMTiles/protomaps(vector)ODbL_firenze.pmtiles>). First, let's inspect the metadata of the PMTiles archive.

```{code-cell} ipython3
url = "https://protomaps.github.io/PMTiles/protomaps(vector)ODbL_firenze.pmtiles"
metadata = leafmap.pmtiles_metadata(url)
metadata
```

Get the list of layers.

```{code-cell} ipython3
print(f"layer names: {metadata['layer_names']}")
```

Get the layer center.

```{code-cell} ipython3
print(f"center: {metadata['center']}")
```

Get the layer bounds.

```{code-cell} ipython3
print(f"bounds: {metadata['bounds']}")
```

Visualize the layer with the default style.

```{code-cell} ipython3
style = leafmap.pmtiles_style(url, cmap='Set3')
style
```

```{code-cell} ipython3
m = leafmap.Map()
m.add_basemap('CartoDB.DarkMatter')
m.add_pmtiles(
    url,
    name='PMTiles',
    style=style,
    zoom_to_layer=True,
    tooltip=False,
)
m
```

Visualize the layer with a custom style.

```{code-cell} ipython3
m = leafmap.Map()

style = {
    "version": 8,
    "sources": {
        "example_source": {
            "type": "vector",
            "url": "pmtiles://" + url,
            "attribution": 'PMTiles',
        }
    },
    "layers": [
        {
            "id": "buildings",
            "source": "example_source",
            "source-layer": "landuse",
            "type": "fill",
            "paint": {"fill-color": "steelblue"},
        },
        {
            "id": "roads",
            "source": "example_source",
            "source-layer": "roads",
            "type": "line",
            "paint": {"line-color": "black"},
        },
    ],
}

m.add_pmtiles(
    url, name='PMTiles', style=style, zoom_to_layer=True, tooltip=True
)
m
```

### Overture data

+++

You can also visualize [Overture data](https://overturemaps.org/). First, let's inspect the metadata of the PMTiles archive.

```{code-cell} ipython3
url = "https://storage.googleapis.com/ahp-research/overture/pmtiles/overture.pmtiles"
metadata = leafmap.pmtiles_metadata(url)
print(f"layer names: {metadata['layer_names']}")
print(f"bounds: {metadata['bounds']}")
```

Visualize the layer with the default style.

```{code-cell} ipython3
m = leafmap.Map(height='800px')
m.add_basemap('CartoDB.DarkMatter')
m.add_pmtiles(url, name='PMTiles', tooltip=False)
m
```

Visualize the layer with a custom style.

```{code-cell} ipython3
m = leafmap.Map(height='800px')
m.add_basemap('CartoDB.DarkMatter')

style = {
    "version": 8,
    "sources": {
        "example_source": {
            "type": "vector",
            "url": "pmtiles://" + url,
            "attribution": 'PMTiles',
        }
    },
    "layers": [
        {
            "id": "admins",
            "source": "example_source",
            "source-layer": "admins",
            "type": "fill",
            "paint": {"fill-color": "#BDD3C7", "fill-opacity": 0.1},
        },
        {
            "id": "buildings",
            "source": "example_source",
            "source-layer": "buildings",
            "type": "fill",
            "paint": {"fill-color": "#FFFFB3", "fill-opacity": 0.5},
        },
        {
            "id": "places",
            "source": "example_source",
            "source-layer": "places",
            "type": "fill",
            "paint": {"fill-color": "#BEBADA", "fill-opacity": 0.5},
        },
        {
            "id": "roads",
            "source": "example_source",
            "source-layer": "roads",
            "type": "line",
            "paint": {"line-color": "#FB8072"},
        },
    ],
}

m.add_pmtiles(url, name='PMTiles', style=style, tooltip=False)

legend_dict = {
    'admins': 'BDD3C7',
    'buildings': 'FFFFB3',
    'places': 'BEBADA',
    'roads': 'FB8072',
}

m.add_legend(legend_dict=legend_dict)
m
```

### Source Cooperative

[Source Cooperative](https://source.coop) hosts a variety of open geospatial data in PMTiles format. In this example, we will visualize the [Google-Microsoft Open Buildings](https://beta.source.coop/repositories/vida/google-microsoft-open-buildings/description) dataset (193.9 GB). First, let's inspect the metadata of the PMTiles archive.

```{code-cell} ipython3
url = 'https://data.source.coop/vida/google-microsoft-open-buildings/pmtiles/go_ms_building_footprints.pmtiles'
metadata = leafmap.pmtiles_metadata(url)
print(f"layer names: {metadata['layer_names']}")
print(f"bounds: {metadata['bounds']}")
```

```{code-cell} ipython3
m = leafmap.Map(center=[20, 0], zoom=2, height='800px')
m.add_basemap('CartoDB.DarkMatter')
m.add_basemap('Esri.WorldImagery', show=False)

style = {
    "version": 8,
    "sources": {
        "example_source": {
            "type": "vector",
            "url": "pmtiles://" + url,
            "attribution": 'PMTiles',
        }
    },
    "layers": [
        {
            "id": "buildings",
            "source": "example_source",
            "source-layer": "building_footprints",
            "type": "fill",
            "paint": {"fill-color": "#3388ff", "fill-opacity": 0.5},
        },
    ],
}

m.add_pmtiles(
    url, name='Buildings', style=style, tooltip=False
)

html = "Source: <a href='https://beta.source.coop/repositories/vida/google-microsoft-open-buildings/description' target='_blank'>source.coop</a>"
m.add_html(html, position='bottomright')

m
```

```{code-cell} ipython3
m.save('buildings.html')
```

## Local PMTiles

tippecanoe is required to convert vector data to pmtiles. Install it with `mamba install -c conda-forge tippecanoe`.

Download [building footprints](https://github.com/opengeos/open-data/blob/main/datasets/libya/Derna_buildings.geojson) of Derna, Libya.

```{code-cell} ipython3
url = 'https://raw.githubusercontent.com/opengeos/open-data/main/datasets/libya/Derna_buildings.geojson'
leafmap.download_file(url, 'buildings.geojson')
```

Convert vector to PMTiles.

```{code-cell} ipython3
pmtiles = 'buildings.pmtiles'
leafmap.geojson_to_pmtiles(
    'buildings.geojson',
    pmtiles,
    layer_name='buildings',
    overwrite=True,
    quiet=True
)
```

Start a HTTP Sever

```{code-cell} ipython3
leafmap.start_server(port=8000)
```

```{code-cell} ipython3
url = f'http://127.0.0.1:8000/{pmtiles}'
# leafmap.pmtiles_metadata(url)
```

Diplay the PMTiles on the map.

```{code-cell} ipython3
m = leafmap.Map()
m.add_basemap('CartoDB.DarkMatter')
m.add_basemap('SATELLITE')

style = {
    "version": 8,
    "sources": {
        "example_source": {
            "type": "vector",
            "url": "pmtiles://" + url,
            "attribution": 'PMTiles',
        }
    },
    "layers": [
        {
            "id": "buildings",
            "source": "example_source",
            "source-layer": "buildings",
            "type": "fill",
            "paint": {"fill-color": "#3388ff", "fill-opacity": 0.5},
        },
    ],
}

# style = leafmap.pmtiles_style(url)  # Use default style

m.add_pmtiles(url, name='Buildings', show=True, zoom_to_layer=True, style=style)
m
```
