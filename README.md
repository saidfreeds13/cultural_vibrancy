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

## Example Workflow

```python
import geopandas as gpd
import osmnx as ox
from h3.api.numpy_int import polyfill, h3_to_geo_boundary
from shapely.geometry import mapping, Polygon
import numpy as np

# Load cultural locations
loc = gpd.read_file("/content/arts_petr.gpkg").to_crs(32636)

# Get study area boundary
area_gdf = ox.geocode_to_gdf("Петроградская").to_crs(32636)

# Filter locations within area
loc_inside = gpd.sjoin(loc, area_gdf, predicate="within")

# Create hex grid over area
geojson = mapping(area_gdf.geometry.iloc[0])
def swap_coords(geojson):
    coords = geojson['coordinates'][0]
    return {'type': 'Polygon', 'coordinates': [[(lat, lng) for lng, lat in coords]]}
hex_ids = polyfill(swap_coords(geojson), resolution=8)
hex_polygons = [Polygon(h3_to_geo_boundary(h, geo_json=True)) for h in hex_ids]
grid_h = gpd.GeoDataFrame({'h3_index': list(hex_ids)}, geometry=hex_polygons, crs='EPSG:4326').to_crs(32636)

# Assign locations to hexes
locations_on_grid = gpd.sjoin(loc_inside, grid_h, how='inner', predicate='within')

# Calculate diversity indices per hex
def richness(group): return group['Рубрики'].nunique()
richness_p = locations_on_grid.groupby('h3_index').apply(richness)
# ...repeat for other indices...

# Merge results and plot
```

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

## License

MIT License.

---

## Contact

For questions or contributions, please open an issue or contact the repository maintainer.

---

**Happy mapping and analyzing urban cultural diversity!**

[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/47431862/1cf7564a-7bb5-44f3-950f-19181d88e1aa/paste.txt
