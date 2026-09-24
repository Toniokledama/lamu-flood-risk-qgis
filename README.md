# Lamu Flood Risk QGIS

GIS-based flood susceptibility and risk mapping for Lamu County, Kenya using QGIS, terrain, hydrological, land-cover and infrastructure data.

## Project Overview

This project develops a GIS-based flood susceptibility and flood risk assessment for Lamu County, Kenya using QGIS.

The analysis integrates terrain, hydrological, rainfall, land-cover, soil and infrastructure datasets to identify areas susceptible to flooding and assess the exposure of settlements and critical infrastructure.

## Objectives

- Prepare and process spatial datasets for Lamu County.
- Analyse elevation and slope characteristics.
- Derive drainage, flow direction and flow accumulation from a Digital Elevation Model (DEM).
- Assess areas susceptible to riverine, pluvial and coastal flooding.
- Develop a flood susceptibility map.
- Analyse exposure of settlements, roads and infrastructure.
- Produce a final flood risk map.

## Study Area

Lamu County, Kenya.

## Software and Tools

- QGIS
- GRASS GIS processing tools in QGIS
- SAGA GIS processing tools in QGIS
- SRTM 30 m DEM
- Google Earth Engine, where required
- Python, where required

## Methodology

SRTM DEM  
↓  
Prepare 50 km hydrological buffer around Lamu County  
↓  
Clip buffered DEM and reproject to WGS 84 / UTM Zone 37S (EPSG:32737)  
↓  
Hydrological diagnostics and flow accumulation  
↓  
Clip accumulation back to Lamu County  
↓  
Stream thresholding and drainage extraction  
↓  
Slope and terrain analysis  
↓  
Rainfall, land-cover and soil analysis  
↓  
Flood susceptibility mapping  
↓  
Exposure analysis  
↓  
Flood risk mapping

## Current Processing Status

### Completed

- Lamu County boundary prepared and reprojected to EPSG:32737.
- SRTM 30 m DEM loaded.
- Original county-clipped DEM created and reprojected.
- Initial sink filling, flow direction and flow accumulation completed for diagnostic purposes.
- A 50 km hydrological buffer was created around Lamu County to reduce administrative-boundary artefacts.
- Buffered SRTM DEM created and reprojected to EPSG:32737.
- SAGA Fill Sinks (Wang & Liu) was tested on the buffered DEM.
- GRASS `r.watershed` accumulation from the unfilled buffered DEM was retained as the better working hydrology result after comparison.
- Working buffered flow-accumulation raster created: `Lamu_Flow_Accumulation_Buffer50km_RAW_WL`.
- Accumulation clipped back to Lamu County: `Lamu_Flow_Accumulation_Lamu_RAW_WL`.
- Negative-accumulation diagnostic showed approximately 1.17% of valid Lamu cells were negative.
- Absolute accumulation raster created: `Lamu_Flow_Accumulation_Lamu_ABS`.
- A 5,000-cell stream threshold was selected as the working drainage threshold, equivalent to about 4.5 km² contributing area at 30 m resolution.
- Stream mask converted to integer/CELL format, thinned, background converted to NULL, and successfully vectorized.
- Vector drainage network clipped to the county boundary: `Lamu_Drainage_5000_Clipped`.
- Dissolved drainage network created: `Lamu_Drainage_5000_Dissolved`.

### Current Step

Hydrological preprocessing and drainage extraction are complete enough to proceed to the next flood-conditioning factor.

The next main analysis step is **slope derivation and classification** from the projected Lamu DEM.

## Important Processing Decisions

- Large raster datasets remain on the local computer and are not stored in GitHub.
- Hydrology was processed over a 50 km buffer instead of exactly at the county boundary to reduce edge effects.
- The SAGA-filled DEM produced geometric artefacts in the derived flow accumulation, so the GRASS `r.watershed` result from the projected buffered DEM without SAGA filling was retained for the working drainage model.
- A 5,000-cell threshold is currently used for the working stream network.
- The drainage line-merging step was not required for continuing the flood-susceptibility workflow.

## Next Steps

1. Derive slope from the projected Lamu DEM.
2. Reclassify slope into flood-susceptibility classes.
3. Prepare additional conditioning factors such as rainfall, land cover and soil.
4. Standardise the factors for multi-criteria flood-susceptibility analysis.
5. Produce the flood-susceptibility surface.
6. Add settlements, roads, population and critical infrastructure for exposure analysis.
7. Produce the final Lamu County flood-risk map.

## Planned Outputs

- Elevation map
- Slope map
- Drainage map
- Flow accumulation map
- Flood susceptibility map
- Coastal flood susceptibility map
- Infrastructure exposure map
- Final flood risk map

## Project Status

**Work in progress — hydrological preprocessing and drainage extraction completed; terrain-factor analysis is next.**

Detailed processing notes are maintained in [`docs/workflow-progress.md`](docs/workflow-progress.md).
