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

# Lab 1

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/giswqs/geog-414/blob/master/book/labs/lab_01.ipynb)

## Submission requirements

1. Upload a screenshot of your map for each question.
2. Provide a link to your notebook on Colab. See instructions [here](https://geog-414.gishub.org/book/labs/instructions.html).

## Question 1

To complete this question, follow the steps below:

1. Install [leafmap](https://leafmap.org) and create a Jupyter Notebook.
2. Copy and paste the code cell provided below into your notebook.
3. Replace the text `Made by Your Name` with your actual name.
4. If desired, update the logo with your own image.
5. Run the code cell to generate an interactive map.
6. Ensure that your name is visible on the map.
7. Take a screenshot of your map and save it as an image.
8. Submit your screenshot and the link to your Colab notebook.

```{code-cell} ipython3
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
cables = 'https://opengeos.org/data/vector/cables.geojson'
callback = lambda feat: {"color": feat["properties"]["color"], "weight": 1}
m.add_geojson(cables, layer_name="Cable lines", style_callback=callback)
# Display the map
m
```

![](https://i.imgur.com/ZfZCdhL.png)
