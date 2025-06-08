# Cultural Cityscape Diversity calculation 

This project provides a Python-based calculation and visualization of **cultural cityscape diversity** within urban areas. It includes two units of diversity analysis. First, the one via hexagons. Second, the one via buffer zones around residential buildings. At the end of each part of the code for two different units of analysis there is a section for merging and downloading in geopackages the cleaned dataset containing all 5 diversity indexes, allowing for a comparative analysis. 

---

## Code sections

- **1. Libraries:** downloads all the necessary packages.
- **2. Diversity indexes:** Adds formulas for calculating diversity.
- **3. Grid creation:** Creates a hexagonal grid:
  - **Richness** number of unique cultural categories in each hexagon/buffer.
  - **Simpson Diversity Index:** probability that two randomly selected locations belong to different categories in each hexagon/buffer.
  - **Berger-Parker Dominance:** proportion of the most abundant category (values near 1 indicate dominance)
  - **Shannon-Wiener Diversity:** the uncertainty in predicting species identity of a random individual
  - **Shannon-Wiener Equity**
- **4. Cultural cityscape diversity. Hexagons as a unit of calculation**
- **5. Diversity of cultural cityscape. Buildings as a unit of calculation:**
- **6. Results Visualisation:** 
- **7. Descriptive stats:** Creates and saves in png. boxplots and histograms for each diversity index

---

## Installation

**Download the file "Diversity_indexes.ipynb" and open it in python 

## Data inputs needed
- **Cultural locations:** Any geo-file (`arts_petr.gpkg`) with point locations and a **categorical column** (e.g., `Рубрики` for type/category of a cultural location).
- **Study area:** Define the area (`"Петроградская"`) for boundary extraction.
---

## Usage

1. **Load and Prepare Data**
    - Import the required libraries.
    - Load the cultural locations file and reproject to a suitable CRS (e.g., EPSG:32636).
    - Retrieve the study area's boundary using OSMnx and reproject as needed.
    - Spatially join points to the area boundary to filter relevant locations.

2. **Create Hexagonal Grid**
    - Use the H3 library to create a hexagonal grid (resolution can be adjusted).
    - Convert H3 indices to polygons and create a GeoDataFrame.
    - Reproject the grid to match your data.

3. **Assign Locations to Grid**
    - Spatially join cultural locations to hex grid cells.

4. **Calculate Diversity Indices**
    - For each hex cell, calculate:
        - **Richness:** Number of unique categories.
        - **Simpson Diversity:** 1 minus the sum of squared proportions.
        - **Berger-Parker Dominance:** Maximum proportion of a single category.
        - **Shannon-Wiener Diversity:** Entropy-based diversity.
        - **Shannon-Wiener Equity:** Evenness of diversity.

5. **Visualize Results**
    - Merge diversity results back to the grid.
    - Plot maps using `matplotlib` and `contextily` for each diversity index.
    - Optionally, use `folium` for interactive web maps.

---

## Customization

- **Grid Resolution:** Adjust the H3 resolution for finer or coarser grids.
- **Category Column:** Change `Рубрики` to match your dataset.
- **Area of Interest:** Use any city or district supported by OSMnx.
---
