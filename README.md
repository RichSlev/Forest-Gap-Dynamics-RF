# Forest Gap Mapping and Gap Dynamics via Machine Learning (Random Forest, R)
Repository for a collaborative remote sensing project focused on the development and evaluation of Random Forest–based methods for mapping forest canopy gaps and gap dynamics from satellite image time series, conducted in collaboration with WSL (2024). The project maps forest canopy gaps from a multi-year satellite image time series and classifies **gap dynamics** between consecutive years (new vs expanding gaps).

## Overview
Many study sites only have a reference gap map for a single year. This workflow uses that reference year to train a **Random Forest** classifier and then generates annual gap maps across the full satellite time series. Annual maps are compared year-to-year to quantify gap dynamics.

## Methods summary
### 1) Annual gap map generation (classification)
- **Training labels:** gap vs non-gap derived from the reference gap map (polygon) for a single year.
- **Predictors:** satellite imagery for each year in the time series (e.g., surface reflectance bands and/or derived indices, depending on preprocessing).
- **Model:** `randomForest` classification run independently for each year’s image to produce annual binary gap maps.

### 2) Gap dynamics classification (change logic)
Annual gap maps are compared between consecutive years to classify gap dynamics:
- **New gap:** a gap pixel that appears in year *t* but was non-gap in year *t-1*.
- **Expanding gap:** a gap pixel in year *t* that is adjacent to an existing gap footprint from earlier years.
Adjacency is evaluated using a neighborhood operation (e.g., `focal`) to identify spatial expansion from prior gaps.

## Required data
To run the R workflow you will need:
- **Reference gap map (polygon)** for a single year (gap delineation).
- **Time series satellite imagery (rasters)** for multiple years covering the same area and aligned to a consistent grid/projection.

If the imagery stack is not already harmonized, ensure:
- identical CRS and resolution across years
- consistent spatial extent and alignment (no pixel shifts)
- consistent masking (cloud, shadow, snow where relevant)

## Repository usage
1. Clone the repository.
2. Set input and output directories in the scripts (paths are currently user-specific).
3. Run the workflow scripts to:
   - generate annual gap maps
   - compute and export gap dynamics layers and summary outputs

## Outputs
Typical outputs include:
- annual **binary gap maps** (one raster per year)
- **gap dynamics** rasters per year-to-year transition (e.g., new vs expanding)
- optional summary statistics (gap area, new gap area, expansion area per year)

## Notes and limitations
- Classification quality is sensitive to how training samples are drawn from the reference polygons and how class imbalance is handled.
- If sensor characteristics differ across years (or if preprocessing is inconsistent), year-specific models may not be directly comparable without harmonization.
- The “expanding” class is adjacency-based and may require tuning of neighborhood size depending on pixel resolution and ecological interpretation.

## Credits and supervision
The WSL Summer School on Old Growth Forest Research is co-organized by:
- Swiss Federal Research Institute for Forest, Snow and Landscape Research (WSL), Birmensdorf, Switzerland
- Bern University of Applied Sciences (HAFL), Zollikofen, Switzerland
- Ukrainian National Forestry University of Ukraine (UNFU), Lviv, Ukraine
- Carpathian Biosphere Reserve (CBR), Rakhiv, Ukraine

Group project contributors:
- Richard Slevin 
- Maria Adelia Widijanto
- Thu Uyen Bui
- Anne Huber

Supervision:
- Marius Rüetschi
- M. Hobi

## Reference
WSL Summer School on Old Growth Forest Research (2024): https://www.wsl.ch/en/events-and-courses/summer-school-old-growth-forest-research/
