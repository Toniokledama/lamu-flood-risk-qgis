# Lamu County Flood Risk Analysis — Workflow Progress

## Current Checkpoint

The project has completed the main hydrological preprocessing and working drainage extraction stages in QGIS. The next major flood-conditioning factor is slope.

## Completed Processing

1. Prepared the Lamu County study boundary.
2. Reprojected the county boundary to WGS 84 / UTM Zone 37S (EPSG:32737).
3. Loaded the SRTM 30 m Digital Elevation Model (DEM).
4. Created an initial county-clipped DEM and reprojected it to EPSG:32737.
5. Performed an initial sink-fill and flow-direction workflow for diagnostic purposes.
6. Created an initial flow-accumulation raster and identified county-boundary effects.
7. Created a 50 km hydrological buffer around Lamu County in EPSG:32737.
8. Reprojected the buffer to EPSG:4326 for clipping the original SRTM raster.
9. Created `Lamu_DEM_Buffer50km_Raw` from the original SRTM dataset.
10. Reprojected the buffered DEM to EPSG:32737 as `Lamu_DEM_Buffer50km_UTM37S`.
11. Tested SAGA Fill Sinks (Wang & Liu) on the buffered DEM.
12. Compared GRASS `r.watershed` accumulation from the SAGA-filled DEM against accumulation from the unfilled projected buffered DEM.
13. Retained the unfilled-buffer GRASS result as the better working hydrology raster because the SAGA-filled version showed strong geometric artefacts.
14. Created `Lamu_Flow_Accumulation_Buffer50km_RAW_WL`.
15. Clipped the working accumulation raster back to Lamu County as `Lamu_Flow_Accumulation_Lamu_RAW_WL`.
16. Created a negative-accumulation mask. The mean was approximately 0.01166497, indicating about 1.17% of valid Lamu cells had negative GRASS accumulation values.
17. Created `Lamu_Flow_Accumulation_Lamu_ABS` using the absolute magnitude of the accumulation raster for stream thresholding.
18. Selected a 5,000-cell working stream threshold.
    - 30 m × 30 m cell = 900 m².
    - 5,000 cells ≈ 4.5 km² contributing area.
19. Created `Lamu_StreamMask_5000`.
20. Converted the stream mask to integer/CELL-compatible format.
21. Applied GRASS `r.thin`.
22. Converted background value 0 to NULL using GRASS `r.null` and thinned again.
23. Successfully converted the stream raster to vector lines using GRASS `r.to.vect`.
24. Saved the vector drainage network and clipped it to Lamu County as `Lamu_Drainage_5000_Clipped`.
25. Created `Lamu_Drainage_5000_Dissolved`.
26. A length-based cleanup attempt was rejected because the raster-to-vector process produced many short database segments even where the mapped stream was visually continuous.
27. Line merging was investigated but is not required for the current flood-susceptibility workflow.

## Current Working Layers

Key hydrology layers to retain include:

- `Lamu_DEM_Buffer50km_UTM37S`
- `Lamu_Flow_Accumulation_Buffer50km_RAW_WL`
- `Lamu_Flow_Accumulation_Lamu_RAW_WL`
- `Lamu_Flow_Accumulation_Lamu_ABS`
- `Lamu_StreamMask_5000`
- `Lamu_Drainage_5000_Clipped`
- `Lamu_Drainage_5000_Dissolved`
- `Lamu_Boundary_UTM37S`

Diagnostic/test layers should be retained until the workflow is fully validated, but they are not necessarily final model inputs.

## Important Methodological Notes

- Hydrology should not be computed strictly inside an administrative boundary when significant upstream terrain lies outside that boundary. A 50 km working buffer was therefore introduced to reduce edge artefacts.
- The 50 km buffer is adequate for local drainage/susceptibility modelling, but it does not represent the full upstream catchment of every major river entering Lamu County.
- GRASS `r.watershed` negative accumulation values indicate potentially incomplete upstream contribution from outside the processing region; they are not automatically invalid data.
- Only about 1.17% of valid county cells were negative after buffered processing, so the 50 km buffer was accepted for the local drainage model.
- The SAGA-filled DEM produced conspicuous geometric flow patterns and was not selected as the working accumulation basis.
- The 5,000-cell threshold is a working drainage threshold rather than a universal hydrological constant. It can be revisited if cartographic or modelling needs change.
- Large DEM and raster products remain local and are not uploaded to GitHub.

## Current QGIS Step

Hydrological preprocessing and drainage extraction have reached a usable checkpoint.

### Next main operation

Derive **slope** from the projected county DEM, using EPSG:32737 and 30 m terrain data.

Suggested output:

`Lamu_Slope_Degrees.tif`

After slope generation, inspect the distribution and classify slope into flood-susceptibility classes before combining it with the other conditioning factors.

## Immediate Next Workflow

1. Organize current local project outputs into a stable folder structure.
2. Derive the slope raster.
3. Inspect slope statistics and select appropriate classes for Lamu's generally low-relief terrain.
4. Prepare rainfall, land-cover and soil layers.
5. Standardise/reclassify all flood-conditioning factors.
6. Combine factors into a flood-susceptibility model.
7. Add exposure layers such as settlements, roads, population and critical infrastructure.
8. Produce final flood-risk products.
