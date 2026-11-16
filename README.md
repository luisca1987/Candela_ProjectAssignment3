# Gulf of Alaska Oceanographic Data Cleaning

**ENV 620 – Big Data in Environmental Science**  
**Week 3 Course Project: The Art of Data Cleaning**  
**Author:** Luis Candela  
**Date:** November 15, 2026

## Project Overview

This repository contains a cleaned oceanographic dataset derived from the Copernicus GLORYS12V1 global ocean reanalysis for January 2017.

The project focuses on preparing oceanographic data from the Gulf of Alaska for subsequent spatial analysis of ocean circulation and environmental variability. This work supports a broader research interest in understanding how large-scale climate variability, including ENSO, influences oceanographic conditions near Kodiak Island and the Gulf of Alaska.

## Data Source

- **Product:** Copernicus GLORYS12V1 Global Ocean Reanalysis
- **Temporal coverage:** January 2017 monthly mean
- **Geographic extent:** 54°–62°N, 160°–130°W
- **Vertical coverage:** Surface layer
- **Original format:** NetCDF (.nc)
- **Cleaned format:** CSV (.csv)

The original NetCDF dataset was preserved without modification.

## Dataset Variables

| Variable | Description | Units |
|---|---|---|
| `temperature_C` | Sea surface temperature | °C |
| `salinity_1e-3` | Surface salinity | Original dataset units |
| `eastward_velocity_m_s` | Eastward ocean current | m/s |
| `northward_velocity_m_s` | Northward ocean current | m/s |
| `sea_surface_height_m` | Sea surface height | m |
| `latitude` | Geographic latitude | Degrees north |
| `longitude` | Geographic longitude | Degrees east |
| `time` | Original monthly mean timestamp | UTC |
| `date` | Standardized date | YYYY-MM-DD |
| `data_status` | Record completeness classification | Categorical |

Five additional Boolean columns identify observations outside the 1.5 × IQR thresholds for each environmental variable.

## Data Inspection

The initial inspection identified:

| Metric | Result |
|---|---:|
| Original regional grid cells | 35,017 |
| Fully missing grid cells | 14,882 |
| Retained oceanographic records | 20,135 |
| Complete records | 20,121 |
| Records with missing velocity values | 14 |
| Duplicate coordinate/time records | 0 |

Spatial inspection showed that missing surface temperature values primarily followed the regional coastline and land areas, consistent with intentional land masking.

## Cleaning Methodology

### 1. Regional and Surface Extraction

Selected the Gulf of Alaska region and extracted the surface layer for temperature, salinity, and current velocity, along with sea surface height.

### 2. Missing Data

Excluded 14,882 grid cells where all five selected environmental variables were missing.

Retained 14 records with missing eastward and northward velocity values. These records were flagged rather than removed because temperature, salinity, and sea surface height remained available.

No numerical imputation was performed.

### 3. Outlier Detection

Applied the interquartile range (IQR) method:

**Lower threshold = Q1 − 1.5 × IQR**  
**Upper threshold = Q3 + 1.5 × IQR**

| Variable | Flagged observations |
|---|---:|
| Temperature | 1,073 |
| Salinity | 1,126 |
| Eastward velocity | 1,158 |
| Northward velocity | 1,387 |
| Sea surface height | 0 |

All outliers were retained because extreme values may represent physically meaningful oceanographic conditions, including coastal freshwater influence, strong currents, and regional temperature gradients.

### 4. Standardization

- Renamed variables using descriptive names and units.
- Added a standardized date column.
- Preserved the original monthly timestamp.
- Added Boolean IQR outlier flags.
- Added a `data_status` column identifying incomplete velocity records.
- Verified that no duplicate coordinate/time records remained.

## Data Quality Considerations

GLORYS12V1 is a previously processed ocean reanalysis product rather than raw sensor measurements.

Consequently, extensive numerical corrections were unnecessary. The cleaning process emphasized transparency, preservation of physically plausible values, and preparation for regional analysis.

Missing land-masked cells were distinguished from potentially incomplete ocean observations.

## Limitations

- The dataset represents one monthly mean, not a multiyear time series.
- The extracted dataset contains only surface conditions.
- IQR flags indicate statistical extremes, not necessarily erroneous measurements.
- The 14 missing velocity records were retained without imputation.
- The dataset cannot independently establish ENSO-related temporal variability or signal propagation.

## Repository Contents

- `GULF_OF_ALASKA_CLEANED_20261115.csv` — Cleaned oceanographic dataset.
- `README.md` — Project documentation.

## Future Work

Future analysis may include mapping sea surface temperature, salinity, and circulation patterns; calculating current speed and direction; examining spatial gradients; and comparing oceanographic conditions across different ENSO periods using additional monthly datasets.

## Tools

Python, xarray, pandas, NumPy, and Matplotlib.

## Acknowledgment

Original oceanographic data provided through the Copernicus Marine Service GLORYS12V1 ocean reanalysis product.
