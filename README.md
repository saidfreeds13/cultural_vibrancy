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
  - **Shannon-Wiener Equity:** neasures how evenly locations are distributed among species, with a value of 1 indicating perfect evenness and values closer to 0 indicating dominance by one or a few species
- **4. Cultural cityscape diversity. Hexagons as a unit of calculation**
- **5. Diversity of cultural cityscape. Buildings as a unit of calculation:**
- **6. Results Visualisation:** 
- **7. Descriptive stats:** Creates and saves in png. boxplots and histograms for each diversity index

---

## Installation

**Download the file "Diversity_indexes.ipynb"** and open it in python 

## Data inputs needed
- **Cultural locations:** Any geo-file (`arts_petr.gpkg`) with point locations and a **categorical column** (e.g., `Рубрики` for type/category of a cultural location).
- **Study area:** Define the area (`"Петроградская"`) for boundary extraction.
---

---

## Adaptation for a user's input 

- **Grid Resolution:** Adjust the the grid resolution.
- **Category Column:** Change `Рубрики` to match your dataset structure.
- **Area of Interest:** Use any city or district supported by OSM.
---
