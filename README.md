# README – Satellite-Derived Bathymetry (SDB) Pipeline for Paranaguá Bay
Autor: Arcelia Haylem Portilla Diaz
https://doi.org/10.5281/zenodo.21688143
## Overview

This repository contains a complete Python pipeline for Satellite-Derived Bathymetry (SDB) applied to the Paranaguá Bay region (Brazil). It integrates Google Earth Engine (GEE), tide correction using the TPXO10 model, machine learning models, and extensive visualization and validation tools.

The pipeline was developed as part of a thesis project by **Portilla Arcelia** and is designed to run in **Google Colab** (but can be adapted to other environments).

---

## Table of Contents
- [Overview](#overview)
- [Pipeline Steps](#pipeline-steps)
- [Requirements](#requirements)
- [Input Data Requirements](#input-data-requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Modules and Classes](#modules-and-classes)
- [Output Structure](#output-structure)
- [Validation and Case Studies](#validation-and-case-studies)
- [Notes](#notes)
- [Author](#author)

---

## Overview

The pipeline performs the following main tasks:

1. **Earth Engine data extraction** – Sentinel-2 imagery, index calculation (NDWI, NDCI, NDTI), and point sampling.
2. **Tide correction** – Using the TPXO10 global tide model to correct depth measurements for tidal fluctuations.
3. **Data loading and preprocessing** – Loading corrected CSV files, converting DN to reflectance, classifying clear water with Gaussian Mixture Models (GMM), and calibrating with *in situ* data (Braza).
4. **Machine learning modeling** – Training and evaluating several SDB models: Lyzenga, Stumpf, Random Forest, Gradient Boosting, and XGBoost.
5. **Evaluation** – Metrics such as RMSE, MAE, R², Bias, and TVU (Total Vertical Uncertainty) according to IHO S-44.
6. **Visualization** – Depth maps, real vs. predicted plots, error distributions, bathymetric profiles, and cross-validation comparisons.
7. **Case analysis** – Comparing different water correction strategies (no correction, GMM with NDCI only, NDTI only, combined, and with Braza calibration).

---

## Pipeline Steps

The main pipeline is orchestrated by the `SDBPipeline` class and consists of 8 steps:

1. **Data loading** – Reads corrected CSV files from Google Drive.
2. **Preprocessing** – Reflectance conversion, index calculation, water clarity classification (GMM), and optional Braza calibration.
3. **Train/test split** – Splits data for model training and evaluation.
4. **Model training** – Trains Lyzenga, Stumpf, Random Forest, Gradient Boosting, and XGBoost.
5. **TVU analysis** – Evaluates vertical uncertainty against IHO S-44 orders.
6. **Visualization** – Generates depth maps, comparison plots, error distributions, etc.
7. **Bathymetric profiles** – Extracts and plots profiles across the study area.
8. **Saving results** – Saves DataFrames, models, and plots to Google Drive.

Additionally, there are scripts for:
- Tide correction using TPXO10 (`h_tpxo10.v2.nc`).
- Cross-validation between Paranaguá and Antonina.
- Comparative analysis of five water correction cases.

---

## Requirements

- Python 3.7+
- Google Colab (recommended) or Jupyter Notebook
- Google Earth Engine account (for GEE routines)
- Google Drive (for data storage and outputs)
- Libraries:
  - `earthengine-api`
  - `geemap`
  - `ipywidgets`
  - `xarray`
  - `scipy`
  - `scikit-learn`
  - `xgboost`
  - `pandas`
  - `numpy`
  - `matplotlib`
  - `seaborn`
  - `joblib`
  - `gspread_dataframe` (optional, for Google Sheets)

---

## Input Data Requirements

The model requires the following primary inputs:

* **Bathymetric data**:
  * Format: tabular or raster
  * Required fields:
    * Geographic coordinates (latitude, longitude)
    * Depth values (meters)
* Coordinate system:
  * Geographic (e.g., WGS84)

Ensure consistency in spatial reference systems across all datasets before processing.

### Tidal Model Considerations

This workflow integrates the TPXOv2 tidal model for sea level and tidal constituent estimation.

**Important**:
* TPXOv2 is **not openly distributable without authorization**.
* Users must obtain **explicit permission from the model authors** prior to usage.
* The repository does **not include TPXOv2 data files**.

### Execution Environment

**Default Configuration (Cloud-Based)**

The routine was designed to run in:
* Google Colab
* Integrated with:
  * Google Drive (personal storage)
  * Google Earth Engine (GEE)

Key characteristics:
* Authentication via personal credentials
* Direct access to cloud-hosted datasets
* Predefined paths linked to the developer’s environment

**Running with Personal Data**

If you intend to execute this workflow using your own datasets:

1. **Update file paths**:
   * Replace all hardcoded links (Google Drive / GEE assets)
   * Ensure local or cloud paths point to your datasets

2. **Modify authentication**:
   * Replace credentials associated with:
     * Google Drive access
     * GEE project environment

3. **Verify data structure compatibility**:
   * Ensure your input data matches expected formats and schema

### Google Earth Engine (GEE) Integration

This workflow includes access to:
* Google Earth Engine assets
* A specific project workspace (originally linked to the developer’s account)

**Important Notes**:
* The current configuration references a **private GEE project**
* Users must:
  * Authenticate with their own GEE account
  * Replace asset IDs and project paths with their own resources

### Limitations and Recommendations

* Hardcoded paths and credentials must be adapted before reuse
* TPXOv2 access is restricted and must be handled externally
* Notebooks (`.ipynb`) may be difficult to version-control:
  * Consider exporting to `.py` scripts for reproducibility
* Ensure consistency between bathymetric resolution and tidal model grid

### Reproducibility Notes

To ensure reproducibility:
* Document all preprocessing steps
* Maintain version control of scripts and data transformations
* Avoid embedding large datasets directly in the repository
* Clearly specify coordinate systems and vertical datums

### Contact / Usage

This code was developed for research purposes.
If you intend to reuse or extend the workflow:
* Adapt paths and credentials
* Ensure compliance with third-party data/model licenses

---

## Installation

In a Colab notebook, run:

```bash
!pip install earthengine-api geemap ipywidgets xgboost gspread_dataframe
```
---

## Author 
For questions or collaborations, please open an issue or contact the author.
Send a private message. email: haylemjoong@gmail.com
