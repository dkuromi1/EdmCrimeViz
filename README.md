# Edmonton Crime Spatial Analysis

**Author:** Darryl Kuromi | **Course:** GIS502 | **Date:** March 31, 2026

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/dkuromi1/EdmCrimeViz/HEAD?urlpath=%2Fdoc%2Ftree%2F01_data_processing.ipynb)
[![Dashboard](https://img.shields.io/badge/Render-Dashboard-blueviolet?style=flat&logo=jupyter)](https://mybinder.org/v2/gh/dkuromi1/EdmCrimeViz/main?urlpath=voila%2Frender%2F01_data_processing.ipynb)

## Project Overview
This project analyzes **250,000+ historical crime records** (2023–2026) from the Edmonton Police Service. While traditional "High Crime" maps often highlight busy commercial areas, this analysis uses a weighted harm index based on the established principles (Cambridge Crime Harm Index) to identify the residential neighborhoods facing the highest relative risk. By moving beyond raw incident counts, this analysis provides a more nuanced understanding of urban safety, revealing that Edmonton's crime distribution is heavily skewed by retail "noise" like shoplifting.

Traditional GIS platforms (ArcGIS Pro/Online) struggled with the computational load and credit costs associated with analysing this dataset. This project solves that bottleneck by using a code-first approach with **GeoPandas**, **Kepler.gl**, and a custom crime severity index based on established principles.

## Interactive Deployment
You can view this project in two ways:
* **[Presentation Dashboard](https://mybinder.org/v2/gh/dkuromi1/EdmCrimeViz/main?urlpath=voila%2Frender%2F01_data_processing.ipynb):** A clean, executive-level view of the maps and insights using Binder and Voilà.
* **[Interactive Notebook](https://mybinder.org/v2/gh/dkuromi1/EdmCrimeViz/main?labpath=01_data_processing.ipynb):** View the full ETL pipeline, spatial joins, and feature engineering logic.

## Technical Architecture
* **Python 3.11** 
* **ETL Pipeline:** Handled multi-CRS projections (converting EPSG:3776 and EPSG:3857 to unified EPSG:4326).
* **GeoPandas** used for point-in-polygon joins and metric area calculations (using EPSG:3776 for Alberta-specific metric math).
* **Apache Parquet** used for storage, significantly reducing memory overhead compared to CSV.
* **Visual Stack:** Synchronized Dual Maps **Folium** and a GPU-accelerated 3D dashboard via **Kepler.gl**.

## The "Superstore" Effect
A primary finding was the discovery of extreme spatial outliers. Analysis revealed that **7 of the 8 highest-crime intersections in Edmonton** correspond directly to Real Canadian Superstore locations.
* **Hotspot #1:** Kingsway Superstore recorded **3,070 crimes**, 99% of which were "Theft Under $5000".
* Nearly a quarter of city-wide shoplifting occurs in less than 1% of the total landmass, distorting unweighted crime stats.
  
## Key Methodology: The Harm Index
To address this bias, this project utilized AI-assisted feature engineering (Gemini 3.1) to map unique EPS crime types to a severity scale.
1.  **Weighting:** High-harm, low-frequency events (like violent crimes) are given greater mathematical weight than high-frequency retail nuisance crimes.
2.  **Normalization:** Crime is calculated as **Harm-per-Capita**, using 2021 Census population data, to identify the neighborhoods where residents face the highest relative risk.
  
## Repository Structure
- `01_data_processing.ipynb`: The primary analysis pipeline.
- `data/`: Raw and processed data files.
- `kepler_config.json`: Saved state for the 3D dashboard.
- `requirements.txt`: Environment dependencies for Binder.
