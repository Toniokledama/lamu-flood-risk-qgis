# Lamu Flood Risk QGIS

GIS-based flood susceptibility and risk mapping for Lamu County, Kenya using QGIS, terrain, hydrological, rainfall, land-cover and soil data.

## Project Status

**Flood susceptibility modelling is complete.** The project has progressed from DEM preparation and drainage extraction through terrain, rainfall, land-cover and soil processing, AHP weighting, final susceptibility classification, settlement labelling and cartographic presentation.

The next phase is validation/sensitivity assessment and, where required, exposure analysis for settlements, roads, population and critical infrastructure.

## Study Area

Lamu County, Kenya.

## Analysis Grid

- CRS: WGS 84 / UTM Zone 37S (`EPSG:32737`)
- Working resolution: 30 m
- Raster dimensions: 5007 × 3268 cells
- Pixel area: 900 m²

## Software and Tools

- QGIS
- GRASS GIS processing tools in QGIS
- SAGA GIS processing tools in QGIS
- Google Earth Engine
- OpenStreetMap / QuickOSM for settlement reference points

## Flood-Conditioning Factors

The final susceptibility model combines six standardized factors, each scored from 1 to 5:

1. Long-term extreme rainfall (CHIRPS Rx5day, 1991–2025)
2. Distance to drainage
3. Elevation
4. Slope
5. Land use / land cover (Dynamic World 2025)
6. Soil texture (KENSOTER)

## AHP Weights

The working AHP weights used in the weighted overlay are:

| Factor | Weight |
|---|---:|
| Rainfall | 0.2422 |
| Distance to drainage | 0.2422 |
| Elevation | 0.2148 |
| Slope | 0.1348 |
| Land cover | 0.0829 |
| Soil texture | 0.0829 |

The pairwise comparison consistency ratio was approximately **0.0088**, below the conventional 0.10 threshold.

## Final Weighted Overlay

The final continuous AHP susceptibility raster is:

`Lamu_Flood_Susceptibility_AHP_v3.tif`

Observed statistics:

- Minimum: 1.2146
- Maximum: 4.9990
- Mean: 3.0368
- Standard deviation: 0.5485

The continuous surface was classified into five equal-interval susceptibility classes:

1. Very Low
2. Low
3. Moderate
4. High
5. Very High

Final classified raster:

`Lamu_Flood_Susceptibility_Final_Classified.tif`

## Final Susceptibility Results

| Class | Area (km²) | Share of classified area |
|---|---:|---:|
| Very Low | 167.24 | 2.79% |
| Low | 1,537.31 | 25.66% |
| Moderate | 3,063.37 | 51.13% |
| High | 1,167.79 | 19.49% |
| Very High | 55.88 | 0.93% |

**High + Very High:** approximately **1,223.67 km²**, or **20.42%** of the classified area.

The classified area totals approximately **5,991.59 km²**.

## Main Processing Workflow

Lamu County boundary  
↓  
DEM preparation and 50 km hydrological buffer  
↓  
GRASS flow accumulation and drainage extraction  
↓  
Slope and elevation preparation  
↓  
Distance-to-drainage raster  
↓  
CHIRPS Rx5day rainfall processing  
↓  
Dynamic World 2025 land-cover processing  
↓  
KENSOTER soil-texture processing  
↓  
Standardisation to 1–5 susceptibility scores  
↓  
AHP weighted overlay  
↓  
Five-class flood-susceptibility map  
↓  
Settlement labelling and final cartographic layout

## Important Processing Decisions

- Large raster datasets remain on the local computer and are not stored in GitHub.
- Hydrology was processed over a 50 km buffer to reduce administrative-boundary artefacts.
- GRASS `r.watershed` accumulation from the projected buffered DEM was retained after comparison with a SAGA-filled workflow that produced geometric artefacts.
- A 5,000-cell stream threshold was used for the working drainage network, equivalent to about 4.5 km² contributing area at 30 m resolution.
- Permanent-water pixels in the final land-cover susceptibility layer were assigned NoData rather than treated as land susceptibility classes.
- All final model rasters were aligned to the same 30 m grid before weighted overlay.

## Interpretation

This product is a **flood susceptibility map**, not a real-time flood forecast. It identifies locations where terrain, drainage, rainfall, land-cover and soil conditions are comparatively more conducive to flooding. A full flood-risk assessment additionally requires exposure and vulnerability information.

## Repository Scope

GitHub stores the project documentation, methodology, scripts and lightweight outputs. Large DEMs, GeoTIFFs and other heavy GIS datasets remain in the local project workspace.

Detailed processing notes are maintained in [`docs/workflow-progress.md`](docs/workflow-progress.md).

Final susceptibility results are summarized in [`docs/flood-susceptibility-results.md`](docs/flood-susceptibility-results.md).
