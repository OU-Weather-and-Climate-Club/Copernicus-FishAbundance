# Copernicus - Europe's eyes on Earth [copernius.eu](https://cds.climate.copernicus.eu/cdsapp#!/search?type=dataset)


----

## Fish Abundance and Catch Data Analysis

This repository contains a Jupyter notebook and datasets to analyse fish abundance and catch data for the Northwest European Shelf and Mediterranean Sea, spanning the years 2006 to 2098 derived from climate projections.

----

### Data Source

The data is sourced from the Copernicus Climate Data Store, specifically the [fisheries abundance dataset](https://cds.climate.copernicus.eu/cdsapp#!/dataset/sis-fisheries-abundance?tab=overview). 

----

### Focus on Bluefin Tuna Catch Data

While the current repository is centered around the analysis of Bluefin Tuna catch data, the methodologies and code within are designed to be adaptable. 

By following the patterns and practices we have laid out in our examples, we can apply these techniques to a wide array of data available through the Copernicus service or similar platforms. 

We encourage users to take advantage of the groundwork we've laid here to explore the vast Copernicus data repositories. 

----

#### Prerequisites
```
Before running the notebooks, you need to have the following packages installed:

- netCDF4
- xarray
- pandas
- numpy
- matplotlib
- geopandas
- shapely
```

You can install these packages using `pip`:

```bash
pip install netCDF4 xarray pandas numpy matplotlib geopandas shapely
```
----

### Working with the dataset

The notebook includes examples of how to open NetCDF (.nc) files, which store the data. The documentation for the netCDF4 library used can be found here, [NetCDF4 docs](https://unidata.github.io/netcdf4-python/).

```python
import netCDF4 as nc
filename = 'path_to_your_file.nc'
dataset = nc.Dataset(filename)
```

----

### Converting to a Pandas DataFrame

For easier manipulation and analysis, you can convert the data into a pandas DataFrame using xarray:

```python
import xarray as xr
data = xr.open_dataset(filename)
dataframe = data.to_dataframe()
```
----

### Creating a GeoDataFrame for Spatial Analysis

In our Jupyter notebook, we demonstrate how to enhance the analysis of Copernicus data by leveraging the capabilities of GeoPandas.

By converting our Pandas DataFrame into a GeoDataFrame, we can attach spatial information to each data point, enabling us to perform sophisticated spatial analyses.

Here's an overview of the process:

1. **GeoDataFrame Creation**: We create a GeoDataFrame from our existing Pandas DataFrame by assigning a `Point` geometry to each record, derived from their respective longitude and latitude values.

2. **Setting the Coordinate Reference System (CRS)**: We define the GeoDataFrame's CRS to WGS84, which is a standard geographical coordinate system that allows for global latitude and longitude measurements.

3. **Spatial Operations**: With a GeoDataFrame, you can perform spatial joins, overlay operations, and much more. This allows for a rich and meaningful exploration of the data, such as identifying clusters of fish abundance or integrating additional spatial layers like marine protected areas.

4. **Visualization**: GeoPandas also enables us to create insightful visualizations with geographic context right out of the box, offering a direct way to plot our data on maps.

5. **Exporting GeoData**: The GeoDataFrame can be easily exported to various geographic file formats, such as GeoJSON, for use with other GIS software or web applications.

Here's a code snippet from the notebooks that illustrates how we convert a DataFrame into a GeoDataFrame:

```python
import geopandas as gpd
from shapely.geometry import Point

# Create a new column 'geometry' with Point objects from the latitude and longitude
bluefin_df['geometry'] = bluefin_df.apply(lambda row: Point(row['longitude'], row['latitude']), axis=1)

# Convert the pandas DataFrame with a 'geometry' column into a GeoDataFrame
geo_tuna = gpd.GeoDataFrame(bluefin_df, geometry='geometry')

# Set the CRS for the GeoDataFrame to WGS84 (lat/long)
geo_tuna.set_crs(epsg=4326, inplace=True)
```

This approach is just one example of how .nc (NetCDF) data, common in climate and oceanographic research, can be enriched with geospatial functionality. We aim to extend this methodology to develop a pipeline for accessing and analyzing similar datasets remotely, streamlining the process to avoid the need for manual downloads. The work is in progress, and we invite collaborators to join us in refining and expanding this pipeline.

----


