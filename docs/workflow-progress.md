# Lamu County Flood Risk Analysis — Workflow Progress

## Current Checkpoint

The project is currently at the hydrological preprocessing stage in QGIS.

### Completed processing

1. Prepared the Lamu County study boundary.
2. Loaded the SRTM 30 m Digital Elevation Model (DEM).
3. Clipped the DEM to Lamu County.
   - Output: `Lamu_DEM_Clipped_Raw`
4. Reprojected the DEM to WGS 84 / UTM Zone 37S.
   - CRS: EPSG:32737
   - Output: `Lamu_DEM_30m_UTM37S`
5. Filled depressions/sinks to create a hydrologically conditioned DEM.
   - Output: `Lamu_DEM_Filled`
6. Generated the flow-direction raster.
   - Output: `Lamu_Flow_Direction_WL`

## Current QGIS Step

The next operation is **flow accumulation** using GRASS GIS `r.watershed` in the QGIS Processing Toolbox.

Current elevation input:

`Lamu_DEM_Filled [EPSG:32737]`

Planned flow-accumulation output:

`Lamu_Flow_Accumulation_WL.tif`

At this checkpoint, flow accumulation has **not yet been completed**.

## Immediate Next Workflow

After flow accumulation:

1. Inspect the flow-accumulation raster and its value distribution.
2. Select an appropriate accumulation threshold for drainage extraction.
3. Extract the drainage/stream network.
4. Generate slope from the projected DEM.
5. Prepare additional flood-conditioning factors such as rainfall, land cover and soil.
6. Standardise/reclassify the factors for flood-susceptibility analysis.
7. Combine the factors into a flood-susceptibility model.
8. Add exposure layers such as settlements, roads and critical infrastructure.
9. Produce the final Lamu County flood-risk map.

## Processing Notes

- Terrain/hydrological calculations use the projected DEM in EPSG:32737 so distance and terrain measurements are handled in metres rather than geographic degrees.
- Hydrological operations are based on `Lamu_DEM_Filled`, not the raw clipped DEM.
- Large raster outputs should generally remain outside GitHub unless intentionally reduced or published as sample/derived products.
