# Lamu Flood Risk QGIS

GIS-based flood susceptibility and risk mapping for Lamu County, Kenya using QGIS, terrain, hydrological, land-cover and infrastructure data.

## Project Overview

This project develops a GIS-based flood susceptibility and flood risk assessment for Lamu County, Kenya using QGIS.

The analysis will integrate terrain, hydrological, rainfall, land-cover, soil and infrastructure datasets to identify areas susceptible to flooding and assess the exposure of settlements and critical infrastructure.

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
- SRTM 30 m DEM
- Google Earth Engine, where required
- Python, where required

## Planned Data

- Digital Elevation Model (DEM)
- Rainfall
- Rivers and drainage network
- Land cover
- Soil
- Roads
- Settlements
- Population
- Critical infrastructure

## Methodology

The general workflow is:

SRTM DEM  
↓  
Clip DEM to Lamu County  
↓  
Reproject to WGS 84 / UTM Zone 37S (EPSG:32737)  
↓  
Hydrological conditioning / sink filling  
↓  
Flow Direction  
↓  
Flow Accumulation  
↓  
Drainage Network  
↓  
Elevation and Slope Analysis  
↓  
Rainfall, Land Cover and Soil Analysis  
↓  
Flood Susceptibility Mapping  
↓  
Exposure Analysis  
↓  
Flood Risk Mapping

## Current Processing Status

### Completed

- Lamu County boundary prepared.
- SRTM 30 m DEM loaded.
- DEM clipped to the Lamu County study area: `Lamu_DEM_Clipped_Raw`.
- DEM reprojected to EPSG:32737 (WGS 84 / UTM Zone 37S): `Lamu_DEM_30m_UTM37S`.
- DEM sinks/depressions filled to produce the hydrologically conditioned raster: `Lamu_DEM_Filled`.
- Flow-direction raster generated: `Lamu_Flow_Direction_WL`.

### Current Step

**Flow accumulation** using GRASS `r.watershed` with `Lamu_DEM_Filled` as the elevation input.

The intended output is:

`Lamu_Flow_Accumulation_WL.tif`

### Next Steps

1. Complete flow accumulation.
2. Extract the drainage/stream network.
3. Derive and classify slope and other terrain factors.
4. Add rainfall, land-cover and soil factors.
5. Develop flood-susceptibility classes.
6. Analyse settlement, road and infrastructure exposure.
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

**Work in progress — hydrological preprocessing is underway.**

Detailed processing notes are maintained in [`docs/workflow-progress.md`](docs/workflow-progress.md).
