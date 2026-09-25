# Lamu County Flood Risk Analysis — Workflow Progress

## Current Checkpoint

The **flood susceptibility stage is complete**. The workflow has progressed from hydrological preprocessing through terrain, rainfall, land-cover and soil preparation, AHP weighting, final classification, settlement labelling and final map layout.

The next analytical phase is validation/sensitivity assessment and, if required, exposure and vulnerability analysis for a full flood-risk product.

## Completed Processing

### 1. Boundary and DEM preparation

1. Prepared the Lamu County study boundary.
2. Reprojected the county boundary to WGS 84 / UTM Zone 37S (`EPSG:32737`).
3. Loaded the 30 m DEM.
4. Created county and buffered DEM products.
5. Created a 50 km hydrological buffer around Lamu County to reduce administrative-boundary artefacts.

### 2. Hydrological preprocessing and drainage

6. Tested SAGA Fill Sinks (Wang & Liu).
7. Compared SAGA-filled and GRASS `r.watershed` flow-accumulation outputs.
8. Retained the GRASS result from the projected buffered DEM because the SAGA-filled workflow produced conspicuous geometric artefacts.
9. Created `Lamu_Flow_Accumulation_Buffer50km_RAW_WL`.
10. Clipped the working accumulation back to Lamu County as `Lamu_Flow_Accumulation_Lamu_RAW_WL`.
11. Created `Lamu_Flow_Accumulation_Lamu_ABS` for stream thresholding.
12. Selected a 5,000-cell stream threshold, equivalent to approximately 4.5 km² contributing area at 30 m resolution.
13. Created and thinned the stream raster.
14. Converted the stream raster to vector lines.
15. Created `Lamu_Drainage_5000_Clipped` and `Lamu_Drainage_5000_Dissolved`.
16. Created the distance-to-drainage susceptibility raster and standardized it to a 1–5 scale.

### 3. Terrain factors

17. Derived slope from the projected DEM.
18. Reclassified slope into flood-susceptibility scores from 1 to 5.
19. Confirmed the correct final slope layer as `Lamu_Slope_Reclassified_FINAL`.
20. Prepared and aligned the elevation susceptibility raster as `Lamu_Elevation_Reclassified_Aligned2`.

### 4. Rainfall factor

21. Used CHIRPS rainfall in Google Earth Engine to derive long-term mean annual Rx5day for 1991–2025.
22. Exported and processed the rainfall surface for Lamu County with a surrounding buffer to reduce coastal gaps.
23. Aligned the final continuous rainfall raster to the 30 m project grid.
24. Reclassified rainfall into five susceptibility classes, with higher Rx5day assigned higher susceptibility.
25. Final rainfall susceptibility raster: `Lamu_Rainfall_Rx5day_Reclassified_v3`.

### 5. Land-cover factor

26. Used Google Dynamic World to derive annual-mode land cover for 2025.
27. Exported the 10 m land-cover raster in `EPSG:32737`.
28. Aligned it to the 30 m reference grid using nearest-neighbour resampling.
29. Reclassified land-cover classes to flood-susceptibility scores.
30. Permanent water was assigned NoData rather than a land susceptibility score.
31. Final layer: `Lamu_LULC_2025_Reclassified_v2`.

### 6. Soil factor

32. Reused the Kenya SOTER / KENSOTER soil dataset.
33. Reprojected KENSOTER to `EPSG:32737`.
34. Clipped soil polygons to Lamu County.
35. Joined the processed topsoil texture lookup table by SUID.
36. Scored soil texture according to relative runoff/infiltration susceptibility:
    - Clay = 5
    - Sandy Clay Loam = 4
    - Clay Loam = 4
    - Loam = 3
    - Sandy Loam = 2
    - Loamy Sand = 1
    - Sand = 1
37. Rasterized the scored soil polygons to the 30 m reference grid.
38. Final layer: `Lamu_Soil_Texture_Reclassified.tif`.

### 7. Raster alignment and standardisation

39. Standardised the six model factors to a common 1–5 susceptibility scale.
40. Confirmed common CRS, extent, origin, resolution and raster dimensions.
41. Final working grid: 5007 × 3268 cells at 30 m resolution in `EPSG:32737`.

### 8. AHP weighted overlay

42. Used the following AHP weights:

| Factor | Weight |
|---|---:|
| Rainfall | 0.2422 |
| Distance to drainage | 0.2422 |
| Elevation | 0.2148 |
| Slope | 0.1348 |
| Land cover | 0.0829 |
| Soil texture | 0.0829 |

43. Pairwise consistency ratio was approximately 0.0088.
44. An early weighted-overlay output was rejected after detecting that a duplicate incorrectly named slope layer contained rainfall-like values.
45. The correct slope raster was renamed `Lamu_Slope_Reclassified_FINAL` and the weighted overlay was rebuilt.
46. Final continuous surface: `Lamu_Flood_Susceptibility_AHP_v3.tif`.
47. Final continuous statistics:
    - Min: 1.2146
    - Max: 4.9990
    - Mean: 3.0368
    - Std. dev.: 0.5485

### 9. Final susceptibility classification

48. Classified the continuous AHP surface into five equal-interval classes:
    - 1 = Very Low
    - 2 = Low
    - 3 = Moderate
    - 4 = High
    - 5 = Very High
49. Final raster: `Lamu_Flood_Susceptibility_Final_Classified.tif`.
50. Final classified raster statistics confirmed valid class values 1–5.

## Final Area Results

| Class | Area (km²) | Percentage |
|---|---:|---:|
| Very Low | 167.24 | 2.79% |
| Low | 1,537.31 | 25.66% |
| Moderate | 3,063.37 | 51.13% |
| High | 1,167.79 | 19.49% |
| Very High | 55.88 | 0.93% |

High + Very High susceptibility covers approximately **1,223.67 km² (20.42%)** of the classified area.

Total classified area is approximately **5,991.59 km²**.

### 10. Settlement reference layer and cartographic presentation

51. Installed and used QuickOSM to retrieve settlement points.
52. Clipped OSM settlements to the Lamu County boundary.
53. Reduced the settlement layer to selected reference places to avoid label clutter.
54. Used a five-class green-to-red susceptibility colour scheme.
55. Created a 4:5 portrait print layout for social-media presentation.
56. Added title, legend, north arrow, scale bar, coordinate grid, settlement labels and author information.
57. Exported the final map as a high-resolution PNG for presentation.

## Important Methodological Notes

- Hydrology was processed over a 50 km buffer instead of strictly within the county boundary to reduce edge effects.
- The buffer does not represent the complete upstream catchment of every river entering Lamu County.
- The 5,000-cell drainage threshold is a modelling choice, not a universal hydrological constant.
- AHP weighting is a multi-criteria decision method and should be accompanied by sensitivity analysis when used in formal research.
- Large rasters remain local and are not committed to GitHub.
- The final product is a **flood susceptibility map**, not a real-time flood forecast.
- A full flood-risk model would additionally require exposure and vulnerability data.

## Next Steps

1. Perform sensitivity analysis on the AHP weights and class thresholds.
2. Compare the susceptibility surface with independent historical flood observations or flood-extent data where available.
3. Document validation limitations clearly.
4. Add roads, population, settlements and critical infrastructure if a full exposure/risk assessment is required.
5. Preserve the final QGIS project, map export and lightweight documentation in the local archive and GitHub repository as appropriate.
