MS Research - Hydrology - Transfer Entropy - Causal Analysis - CAMELS
Author: Prasanna Paudel
Institution: Department of Civil, Construction, and Environmental Engineering, The University of Alabama
Advisor: Dr. Peishi Jiang
Degree: Master of Science in Civil Engineering (Water Resources)
Period: 2025 – 2026
---
What This Research Is About
This repository contains all Jupyter notebooks from my MS research on Transfer Entropy (TE) and information-theoretic causal analysis in hydrology.
The central question is: Why does Transfer Entropy from atmospheric forcings to streamflow vary across basins?
I used the CAMELS dataset, which covers 671 watersheds across the continental United States (CONUS). For each basin, I computed how much information each atmospheric forcing - precipitation (PRCP), solar radiation (SRAD), air temperature (Tair), and vapor pressure (VP) — transfers to streamflow (Q) at daily lags from 1 to 30 days.
Transfer Entropy gives lagged information-flow evidence. It does not prove direct physical causation by itself. All interpretations in this work are supported by CAMELS basin attributes and hydrologic reasoning.
---
Dataset
CAMELS (Catchment Attributes and Meteorology for Large-sample Studies)
671 CONUS basins
Daily time series: PRCP, SRAD, Tair, VP, and streamflow Q
Daymet meteorological forcing
USGS streamflow observations
Basin attributes: climate, hydrology, topography, soil, geology, vegetation
More information: https://ral.ucar.edu/solutions/products/camels
---
TE Method Summary
Transfer Entropy computed as conditional mutual information: I(Q(t); X(t-tau) | Q(t-1))
Lags: tau = 1 to 30 days
Bins: 5 (uniform) for SRAD, Tair, VP; special PRCP binning (1 zero-rainfall bin + 4 positive-rainfall bins)
Significance test: 200-shuffle surrogate test
Outputs: raw TE values, shuffle thresholds, p-values, significant TE results
---
Repository Structure
```
MS-Research-Hydrology-TransferEntropy-CausalAnalysis-CAMELS/
│
├── README.md
│
├── 01_TE_computation/
│   └── ALL_CAMEL_BASINS_FORCING_TO_STREAMFLOW.ipynb
│
├── 02_basin_to_basin_analysis/
│   ├── CAMELS_TE_basin_to_basin_analysis.ipynb
│   ├── TE_basin_to_basin_all_attributes.ipynb
│   └── analysis_of_all_basin.ipynb
│
├── 03_TE_vs_attributes/
│   ├── TE_peak_vs_attributes.ipynb
│   ├── weighted_avg_TE_vs_attributes.ipynb
│   └── Lag1_vs_Attributes.ipynb
│
├── 04_diagnostic_analysis/
│   ├── TE_LongLag_Tair_Diagnost.ipynb
│   └── TE_Northeast_neighbor_analysis.ipynb
│
└── 05_PET_TE_analysis/
    └── TE_PET_Q_all_basins.ipynb
```
---
Notebook Descriptions
01_TE_computation
ALL_CAMEL_BASINS_FORCING_TO_STREAMFLOW.ipynb
Reads raw Daymet forcing and USGS streamflow files for all 671 CAMELS basins. Merges them into clean daily CSVs with Date, PRCP, SRAD, Tair, VP, and Q. Computes Transfer Entropy from each forcing to streamflow for lags 1 to 30 days. Runs 200-shuffle significance tests. Saves raw TE, shuffle thresholds, p-values, and significant TE results. Produces time-series PDFs and CONUS maps. This is the foundation notebook — all other notebooks depend on the outputs generated here.
---
02_basin_to_basin_analysis
CAMELS_TE_basin_to_basin_analysis.ipynb
Loads all CAMELS attribute files (climate, geology, hydrology, soil, topography, vegetation) and merges them into one combined 671-basin attribute table. Joins this with the TE dominant forcing summary to begin basin-to-basin comparison of which atmospheric forcing drives streamflow in each watershed.
TE_basin_to_basin_all_attributes.ipynb
Extended basin-to-basin analysis using 60 CAMELS attributes. Builds a full forcing-wise peak TE and peak lag summary per basin. Creates CONUS maps and scatter plots showing how dominant forcing patterns relate to basin characteristics such as aridity, frac_snow, runoff_ratio, and baseflow_index.
analysis_of_all_basin.ipynb
For each of the 671 basins, ranks which CAMELS attributes best explain why that basin shows its particular dominant forcing and peak lag. Uses Spearman correlation across the full population to measure attribute relevance, and basin-level z-scores to measure how unusual each basin is on each attribute. Produces a master table with one row per basin: dominant forcing, peak TE, peak lag, and the top 10 driving attributes with values and z-scores.
---
03_TE_vs_attributes
TE_peak_vs_attributes.ipynb
Takes the peak TE value and peak lag per forcing per basin. Produces a 52-page PDF with scatter plots of peak TE vs each CAMELS attribute and peak lag vs each CAMELS attribute, across all 4 forcings. Points are colored by lag group. Includes Pearson r values on each plot. First systematic attribute-level analysis of basin-to-basin TE variation.
weighted_avg_TE_vs_attributes.ipynb
Replaces peak TE and peak lag with weighted-average lag (tau-star) and weighted-average TE (TE-star), computed over all statistically significant lags per basin per forcing. Correlates these against 50 CAMELS attributes using Pearson r. Makes scatter plots and CONUS maps. Compares with peak-based results to check whether the choice of TE summary metric changes the attribute story.
Lag1_vs_Attributes.ipynb
Focuses on the shortest timescale: TE at exactly lag 1 day, kept only where it passed the shuffle significance test. Correlates lag-1 TE against 50 CAMELS attributes. Makes scatter plots, CONUS maps, and attribute ranking tables. Compares with peak TE and weighted TE results to understand how the basin-attribute relationships change across timescales.
---
04_diagnostic_analysis
TE_LongLag_Tair_Diagnost.ipynb
Diagnostic investigation of basins that show unusually long Tair TE lags (20 or more days). Computes year-wise Mutual Information heatmaps, lagged Pearson correlation, and time-series comparison plots for 3 selected basins. Checks whether long Tair lags are physically meaningful (linked to snow and seasonal energy cycles) or are artifacts of the data.
TE_Northeast_neighbor_analysis.ipynb
Investigates why nearby basins in the Northeast United States show very different dominant forcings and peak lags despite being geographically close. Computes two robustness metrics per basin: forcing margin (how clearly the top forcing beats the second-best) and lag plateau width (how sharp the TE peak is). Identifies robust vs fragile dominant forcing labels. Compares Northeast basins with the rest of CONUS and links fragile labels to basin attributes.
---
05_PET_TE_analysis
TE_PET_Q_all_basins.ipynb
Extends the TE framework to potential evapotranspiration (PET) as an additional forcing variable. Reads CAMELS Daymet model output files to extract daily PET values. Builds merged daily PET-Q files for all available basins and computes Transfer Entropy from PET to streamflow. Explores how energy-demand information (PET) propagates to streamflow across different basin types.
---
Key Scientific Findings (Summary)
PRCP is the dominant forcing in most basins, especially in humid and steep-terrain basins with fast runoff response.
Tair and SRAD emerge as dominant in energy-limited basins, snow-influenced basins, and some baseflow-dominated systems.
PRCP TE peaks at shorter lags (mean around 8 days). Tair TE peaks at longer lags (mean around 15 days), consistent with slower snowmelt and seasonal energy processes.
Forcing margin analysis shows that for many basins, especially in the Northeast, the dominant forcing label is not highly stable — the top two forcings have similar TE values.
Basin attributes linked to TE variation include aridity, frac_snow, runoff_ratio, baseflow_index, slope_mean, and soil conductivity.
All findings are treated as evidence of information-flow patterns. They support hydrologic hypotheses but do not establish direct physical causation.
---
Dependencies
```
pandas
numpy
matplotlib
seaborn
scipy
cartopy
PyPDF2
pathlib
```
---
Contact
Prasanna Paudel
ppaudel2@crimson.ua.edu
prasannapdl777@gmail.com
University of Alabama, Tuscaloosa, AL
