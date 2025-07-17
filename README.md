# Street View Greenness and Firearm-Related Violence

This repository contains the full analytic workflow for our study:  
**_“Street-View Measures of Urban Greenness, Area-Level Deprivation, and the Risk of Firearm-Related Homicide: A Matched Case-Control Study”_**

We investigate how satellite-derived greenness (NDVI), access to parks, and area deprivation are associated with firearm-related homicide risk in Cook County, Illinois, using spatial data integration and propensity score matching.

---

## 📁 Project Structure

The R code provided performs the following key tasks:

### 1. **Data Acquisition and Cleaning**
- Loads NDVI data (2000, 2010, 2019) by census tract.
- Merges NDVI data with **Area Deprivation Index (ADI)** using the `sociome` R package.
- Cleans and processes Cook County census tract shapefiles.
- Loads and filters firearm homicide records from the Medical Examiner’s dataset.
- Reads in shapefiles for parks and park priority zones.
- Computes spatial joins and ensures geometry validity.

### 2. **Feature Engineering**
- Calculates the distance from each homicide to the nearest park.
- Assigns homicide cases to park priority zones.
- Computes pairwise geographic distances between incidents to flag matches within a 5-mile radius.
- Scales NDVI values and computes longitudinal greenness change from 2010 to 2019.
- Assigns quintiles for ADI, greenness, and distance to park access.

### 3. **Propensity Score Matching**
- Implements nearest-neighbor matching (ratio 4:1) for homicide vs. natural death using the `MatchIt` package.
- Matches on age, sex, race, spatial proximity, and temporal proximity (≤ 45 days).
- Assesses covariate balance with the `cobalt` package using `love.plot()`.

### 4. **Modeling**
- Fits a **conditional logistic regression** model (`clogit`) on matched data, testing associations between homicide risk and:
  - NDVI
  - ADI
  - Park access
  - Transit equity
  - Health indicators (from CDC PLACES data)
- Tests for multicollinearity using Variance Inflation Factors (`car::vif()`).

### 5. **Descriptive Analysis & Visualization**
- Generates comparative boxplots of ADI and NDVI by race and by manner of death.
- Tests for racial differences using Wilcoxon rank-sum tests.
- Plots summary statistics with annotated medians and p-values using `ggplot2` and `ggpubr`.

### 6. **Directed Acyclic Graph (DAG) Modeling**
- Constructs multiple DAGs to visualize assumed causal pathways between NDVI, ADI, health mediators, and firearm homicide using `ggdag`.
- Identifies confounders, mediators, and adjustment sets for inference.

---

## 📦 Key Packages Used

```r
library(dplyr)
library(sf)
library(tigris)
library(sociome)
library(geosphere)
library(MatchIt)
library(cobalt)
library(survival)
library(car)
library(ggplot2)
library(ggpubr)
library(ggdag)
```

### Data Sources
- NDVI Tract-Level Data (2000, 2010, 2019)
- Area Deprivation Index (2019)
- Medical Examiner Case Archive (Firearm-related deaths)
- Cook County Tracts and Parks Shapefiles
- Park Priority Zones (ParkServe)
- CDC PLACES Tract-Level Health Data (2023 release)
- Transit Equity Metrics (Park access, rank, and fare from Chicago)

### Study Focus
Our matched case-control analysis examines whether lower greenness and higher deprivation at the area level are associated with increased odds of firearm-related homicide. Using robust spatial joins, causal DAGs, and regression models, we explore race-specific disparities and structural risk factors underlying firearm violence in urban environments.

### Outputs
- Covariate balance plots and diagnostics
- Stratified boxplots by race and death type
- Conditional logistic regression tables
- Causal DAGs identifying mediators and confounders
- Variance inflation diagnostics for multicollinearity
