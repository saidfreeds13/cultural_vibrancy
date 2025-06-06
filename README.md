# cultural_vibrancy
# Cultural Cityscape Diversity Calculator

This project provides a Python-based workflow for calculating and visualizing **cultural cityscape diversity** within urban areas. It combines geospatial data processing, spatial grid analysis, and multiple diversity indices to help urbanists, researchers, and city planners evaluate the spatial richness and distribution of cultural locations in a city.

---

## Features

- **Automated Data Processing:** Reads and processes geospatial data of cultural locations.
- **Spatial Analysis:** Assigns cultural locations to a hexagonal grid covering the study area.
- **Diversity Metrics:** Calculates several diversity indices per grid cell:
  - **Richness** (number of unique cultural categories)
  - **Simpson Diversity Index**
  - **Berger-Parker Dominance**
  - **Shannon-Wiener Diversity**
  - **Shannon-Wiener Equity**
- **Visualization:** Maps the spatial distribution of diversity metrics using interactive and static plots.

---

## Installation

1. **Clone the repository** (if applicable) or download the notebook and required files.

2. **Install dependencies** (in Colab or locally):
    ```python
    !pip install osmnx mapclassify h3 contextily
    ```

    **Main Python libraries used:**
    - `osmnx`
    - `mapclassify`
    - `h3`
    - `contextily`
    - `geopandas`
    - `pandas`
    - `numpy`
    - `matplotlib`
    - `folium`
    - `seaborn`
    - `shapely`

---

## Data Requirements

- **Cultural Locations File:** A GeoPackage file (e.g., `arts_petr.gpkg`) with point locations and a categorical column (e.g., `Рубрики` for type/category).
- **Study Area:** Defined by a place name (e.g., `"Петроградская"`) for boundary extraction via OSMnx.

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

## Diversity Indices Explained

- **Richness:** Number of unique cultural categories in each hex.
- **Simpson Index:** Probability that two randomly selected locations belong to different categories.
- **Berger-Parker:** Proportion of the most abundant category (values near 1 indicate dominance).
- **Shannon-Wiener:** Measures both abundance and evenness of categories.
- **Shannon-Wiener Equity:** Normalized entropy (evenness).

---

## Customization

- **Grid Resolution:** Adjust the H3 resolution for finer or coarser grids.
- **Category Column:** Change `Рубрики` to match your dataset.
- **Area of Interest:** Use any city or district supported by OSMnx.

---

## Contact

For questions or contributions, please open an issue or contact the repository maintainer.
