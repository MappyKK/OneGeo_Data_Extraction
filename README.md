# One Geo Data Extraction Script for Propeterra

This is a script that extracts building data from One Geo in a GeoJSON format for Propeterra

# Description & Importance

This is a Python and SQL script that extracts building data larger that 30m that fit a certain class of buildings from the One Geo data provider for world buildings.

This script was created by gathering One Geo data through chunking in 0.5 by 0.5 degrees tiles and then combining these extracted tiles into one geojson per country.

# How it's Made

Coding Languages/Libraries/APIs: Python, SQL, DuckDB, GeoPandas, Pandas, Geopy, Nominatim, json, requests, os 

Data Types Supported: GeoJSON, Parquet, other forms of spatial data such as shapefiles or geopackages should work by changing the driver when exporting

Projection Used: EPSG:4326 WGS 84

Output Projection: EPSG:4087 (For visualization in Cesium)

Data Layers used: 

Divisions Area Layer (Administrative Boundaries) From Overture

    s3://overturemaps-us-west-2/release/2025-06-25.0/theme=divisions/type=division_area/*.parquet

Filter: 

Height: 30m +

Building Types: commerce, commerce:office, commerce:retail, residence, other, industry:storage
    
Install duckdb, pyarrow, and geopandas before running

# Script Description:

Utilizes the Divisions Area Layer to clip each building to their corresponding countries by using a bounding box without specifically giving the output layer a country code field

Looped through a dictionary of country codes (two-letter ISO 3166-1 standard) with their corresponding country names to extract data for each target country.

# Gathering Data through Tiling (Chunking):

### Step 1:

Utilizing the country's bounding box split the bounding box into 0.5 by 0.5 degree tiles (chunks) since this is the max that One Geo API calls can support

### Step 2:

Select the tile or parts of it that intersect with the country's boundaries

### Step 3:

Call the One Geo API for every tile that intersects and export it as a geojson

### Step 4:

Append each geojson tile with each other to create one single geojson layer for each country.

### Extra Functionality

Reverse Geocoding, Lat, Long Coordinates from building centroids.
