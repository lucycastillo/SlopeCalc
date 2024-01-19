# Soil Raster to SLC

This program is to sumarize predicted soil types in raster format by Soil Landscapes Canada (SLC) polygons. Output follows the format of Canadian Soil Information Service: SLC 3.2 Component Table.
The Component Table describes each of the individual soil and landscape features which comprise the polygon (i.e. the components of the polygon). 
Each component includes its estimated extent, slope, and local surface form class.

## Table of Contents

- [About](#about)
- [Features](#features)
- [Usage](#usage)

## About

Two distinct scripts operate together to create a comprehensive Excel table that includes soil data such as soil ID, 
component number, component ID, slope median, slope quantiles, and slope class. 

## Features

- Extracts geospatial data from soil raster, slope, DEM, and shapefiles
- Assigns letter grade according to slope percentage
- Summarizes data in comprehensive Excel table

## Usage

  Inputs: soil raster file, SLC polygon shapefile, slope file and DEM. All with same projection : LCC with WGS84 datum.
  Output: CSV or DBF file.

  
