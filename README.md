# Degraded Land Analysis (Google Earth Engine)

This repository contains scripts to identify and analyze degraded land using Google Earth Engine (GEE).
## Demo
<img width="3712" height="3715" alt="BSI_Visual tif jpg" src="https://github.com/user-attachments/assets/a90141c1-6f08-4473-93b0-24bda92d6a4f" />


## Project Files
- `degraded_land_gee.py` — Main processing script that runs the GEE analysis.
- `redme.md` — (legacy) short README kept for compatibility.

## Features
- Load and preprocess satellite imagery
- Apply degradation detection algorithms
- Export results (tables / GeoTIFFs) to Google Drive or Cloud Storage

## Prerequisites
- Python 3.8 or later
- A Google Earth Engine account and the `earthengine` Python package
- Install dependencies (example):

```bash
pip install earthengine-api numpy pandas
```

## Setup
1. Authenticate GEE (one-time):

```bash
earthengine authenticate
```

2. Configure any project-specific settings in `degraded_land_gee.py` (area of interest, date ranges, export paths).

## Usage

```bash
python degraded_land_gee.py
```

## Notes & Tips
- Test with a small AOI and short date range first to iterate quickly.
- Use `--dry-run` or add logging when developing to avoid accidental large exports.

## License
Add an appropriate license (e.g., MIT) in a `LICENSE` file.

## Contact
Open an issue or contact the maintainer for help.

## Theory & Spectral Indices
This section summarizes common vegetation and disturbance indices used in degraded-land analysis, with formulas and recommended bands.

- **Normalized Difference Vegetation Index (NDVI)**: measures vegetation 'greenness' using near-infrared (NIR) and red bands.

	$$\mathrm{NDVI} = \frac{\mathrm{NIR} - \mathrm{Red}}{\mathrm{NIR} + \mathrm{Red}}$$

	Interpretation: values range from -1 to +1; higher values indicate denser, healthier vegetation.

- **Enhanced Vegetation Index (EVI)**: improves sensitivity in high-biomass areas and corrects for atmospheric/soil effects. Typical coefficients: $G=2.5, C_1=6, C_2=7.5, L=1$.

	$$\mathrm{EVI} = G \cdot \frac{\mathrm{NIR} - \mathrm{Red}}{\mathrm{NIR} + C_1\cdot\mathrm{Red} - C_2\cdot\mathrm{Blue} + L}$$

- **Soil-Adjusted Vegetation Index (SAVI)**: reduces soil brightness influence. Commonly $L=0.5$.

	$$\mathrm{SAVI} = \left(\frac{\mathrm{NIR} - \mathrm{Red}}{\mathrm{NIR} + \mathrm{Red} + L}\right)\cdot(1+L)$$

- **Normalized Burn Ratio (NBR)**: used for detecting fire or disturbance by contrasting NIR and shortwave infrared (SWIR).

	$$\mathrm{NBR} = \frac{\mathrm{NIR} - \mathrm{SWIR2}}{\mathrm{NIR} + \mathrm{SWIR2}}$$

### Typical band selections
- Sentinel-2: NIR = B8, Red = B4, Blue = B2, SWIR2 = B12
- Landsat 8: NIR = B5, Red = B4, Blue = B2, SWIR2 = B7

### Practical notes
- Always ensure bands are atmospherically corrected (surface reflectance) when comparing indices across dates or sensors.
- For time-series analysis, mask clouds and shadows, and consider smoothing (e.g., median composites, temporal filters).

