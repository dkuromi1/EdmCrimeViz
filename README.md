# Edmonton Crime Spatial Analysis

**Author:** Darryl Kuromi | **Course:** GIS502 | **Date:** March 31, 2026

[![Quarto](https://img.shields.io/badge/Quarto-447099?style=flat-square&logo=quarto&logoColor=white)](https://edmcrimevizreport.netlify.app/)
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/dkuromi1/EdmCrimeViz/HEAD?urlpath=%2Fdoc%2Ftree%2F01_data_processing.ipynb)

## Project Overview
This project analyzes **250,000+ historical crime records** (2023–2026) from the Edmonton Police Service using open source libraries like GeoPandas and Kepler.gl. It also uses a weighted harm index based on the Cambridge Crime Harm Index, to identify the residential neighbourhoods facing the highest relative risk.

## Interactive Deployment
This project can be viewed in two ways:
* **[Static Quarto Jupyter Notebook Report](https://edmcrimevizreport.netlify.app/):** View a static report with code available.
* **[Interactive Jupyter Notebook](https://mybinder.org/v2/gh/dkuromi1/EdmCrimeViz/main?labpath=01_data_processing.ipynb):** View a live notebook hosted on Binder.

## Run Locally
Create the Conda environment, activate it, and launch JupyterLab:

```bash
conda env create -f environment.yml
conda activate edmonton_crime_viz
jupyter lab
```

Then open `01_data_processing.ipynb` and run the notebook from top to bottom.

Expected output: the pipeline will read the cached raw inputs in `data/raw/`, regenerate processed datasets in `data/processed/`, and render the Folium dual map and final Kepler.gl map inside the notebook.

## Technical Architecture
* **Python 3.11** 
* **ETL Pipeline:** Handled multi-CRS projections (converting EPSG:3776 and EPSG:3857 to unified EPSG:4326).
* **GeoPandas** used for point-in-polygon joins and metric area calculations (using EPSG:3776 for Alberta-specific metric math).
* **Apache Parquet** used for storage, significantly reducing memory overhead compared to CSV.
* **Visual Stack:** Synchronized Dual Maps using **Folium** and a GPU-accelerated 3D dashboard via **Kepler.gl**.

## The "Superstore" Effect
A primary finding was the discovery of extreme spatial outliers. Analysis revealed that **7 of the 8 highest-crime intersections in Edmonton** correspond directly to Real Canadian Superstore locations.
* **Hotspot #1:** Kingsway Superstore recorded **3,070 crimes**, 99% of which were "Theft Under $5000".
* Nearly a quarter of city-wide shoplifting occurs in less than 1% of the total landmass, distorting unweighted crime stats.
  
## Crime Ranking Methodology: The Harm Index
This project uses a custom harm-weighting scheme inspired by the Cambridge Crime Harm Index. EPS incident types were mapped to a severity scale with LLM assistance and then manually reviewed and adjusted to better reflect local offense categories. The resulting scores are used to compare neighbourhood-level harm per capita rather than raw incident counts alone.
