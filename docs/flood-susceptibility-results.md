# Lamu County Flood Susceptibility Results

## Final Model

The final flood-susceptibility model combines six standardized conditioning factors using an AHP weighted overlay:

- Long-term extreme rainfall (CHIRPS Rx5day, 1991–2025)
- Distance to drainage
- Elevation
- Slope
- Land use / land cover (Dynamic World 2025)
- Soil texture (KENSOTER)

All factors were aligned to a common 30 m raster grid in WGS 84 / UTM Zone 37S (`EPSG:32737`) and scored on a common 1–5 susceptibility scale before overlay.

## AHP Weights

| Factor | Weight |
|---|---:|
| Rainfall | 0.2422 |
| Distance to drainage | 0.2422 |
| Elevation | 0.2148 |
| Slope | 0.1348 |
| Land cover | 0.0829 |
| Soil texture | 0.0829 |

Pairwise consistency ratio: approximately **0.0088**.

## Final Continuous Surface

Output:

`Lamu_Flood_Susceptibility_AHP_v3.tif`

Statistics:

- Minimum: 1.2146
- Maximum: 4.9990
- Mean: 3.0368
- Standard deviation: 0.5485

## Final Classified Surface

Output:

`Lamu_Flood_Susceptibility_Final_Classified.tif`

The continuous surface was divided into five equal-interval susceptibility classes:

| Class | Susceptibility | Area (km²) | Percentage |
|---:|---|---:|---:|
| 1 | Very Low | 167.24 | 2.79% |
| 2 | Low | 1,537.31 | 25.66% |
| 3 | Moderate | 3,063.37 | 51.13% |
| 4 | High | 1,167.79 | 19.49% |
| 5 | Very High | 55.88 | 0.93% |

Total classified area: approximately **5,991.59 km²**.

High + Very High susceptibility: approximately **1,223.67 km² (20.42%)**.

## Interpretation

The dominant class is **Moderate**, covering just over half of the classified area. High and Very High susceptibility together account for about one-fifth of the classified area and are concentrated mainly in low-lying and drainage-connected zones.

The map indicates relative susceptibility rather than flood depth, flood timing or probability for a specific storm event. It should therefore be interpreted as a spatial screening and preparedness product rather than a real-time forecast.

## Validation and Research Use

For formal research use, the model should be accompanied by:

- sensitivity analysis of AHP weights and classification thresholds;
- comparison with independent historical flood observations or flood-extent products where available;
- clear documentation of uncertainty and data limitations.

A full flood-risk assessment additionally requires exposure and vulnerability information for settlements, roads, population and critical infrastructure.
