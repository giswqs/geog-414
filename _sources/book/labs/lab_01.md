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

# Lab 1

**Firstname Lastname**

Install [leafmap](https://leafmap.org) and create a Jupyter Notebook. Copy and paste the following code cell to your notebook. Change the text `Made by Your Name` to your name. Update the logo to your own image if you wish. Then run the code cell to generate an interactive map. Note that you name must be visible on the map. Take a screenshot of your map and save it as an image. Upload the screenshot to Canvas.

```{code-cell}
import leafmap
# Creat an interactive map
m = leafmap.Map(center=[20, 0], zoom=2, height='600px')
# Add basemap
m.add_basemap("CartoDB.DarkMatter")
# Add text to the map
text = "Made by Your Name"
m.add_text(text, fontsize=20, position='bottomright')
# Add a logo to the map
logo = 'https://i.imgur.com/at4Qprk.png'
m.add_image(logo, position='bottomright')
# Add GeoJSON data to the map
cables = 'https://open.gishub.org/data/vector/cables.geojson'
callback = lambda feat: {"color": feat["properties"]["color"], "weight": 1}
m.add_geojson(cables, layer_name="Cable lines", style_callback=callback)
# Display the map
m
```

![](https://i.imgur.com/ZfZCdhL.png)
