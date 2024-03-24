# CSky2TopoCrafter: QGIS DEM Downloader & Heightmap Generator
This is CSky2TopoCraft: a QGIS plugin that will allow you to automatically download, format and export realistic heightmaps to the Cities Skylines 2 map editor. It essentially embeds OpenTopography's DEM downloader script into an automation of shanks' tried-and-tested method of heightmap creation, with some elaboration.

You will need:

The Quantum Geographic Information System (QGIS)

The plugin.

The ability to read.

## Limitations
Note that this has been created as a convenience tool for players making heightmaps for cities outside the United States and other informationally-rich countries (e.g., Switzerland). Users will not be able to use it to download and create heightmaps from the USGS 1-metre DEM data product. It is limited to the global (30M+) products normally accessible through the OpenTopography DEM Downloader plugin.

## Instructions

### Install QGIS
Download and install QGIS from https://qgis.org/en/site/forusers/download.html
### Install Plugin
Download the plugin .zip file from this repository. Open QGIS.

In QGIS:

![maiplugin](images/maiplugin.png)

Navigate to the correct tab and hit the ellipsis to find the .zip file you just downloaded. Install plugin.

![insfromzip](images/insfromzip.png)

The plugin will appear listed in the Plugins menu.

### Get OpenTopography API Key
Navigate to https://opentopography.org/ and hit Request an API key to create an account.

![otrequestapikey](images/otrequestapikey.png)

Find your API key at: https://portal.opentopography.org/requestService?service=api

![otapikey](images/otapikey.png)

### Determine your Coordinate Reference System and Coordinates
#### Step 1
Navigate to https://mangomap.com/robertyoung/maps/69585/what-utm-zone-am-i-in-# and find the UTM zone in which your city falls.

![findutm](images/findutm.png)

#### Step 2
Navigate to https://epsg.io/map#srs=4326&x=0.000000&y=0.000000&z=1&layer=streets and hit change. 

![changecrs](images/epsgchangecrs.png)

Enter your UTM zone in the search box and use the results to change the coordinate reference system (CRS) to match your UTM zone. It must be WGS 84 / UTM zone (number)(hemisphere).

![epsgselectcrs](images/epsgselectcrs.png)

Use the map cross-hairs (2) to determine the coordinates (3) of your city. Note down the EPSG code for your CRS (1).

![findcoord](images/findcoord.png)
### Using the Plugin
### Advanced
#### Download and Merge SRTM15+ Source Bathymetry Data
The user can choose to automatically download and merge SRTM15+ source bathymetry data with the DEM. The script initiates the OpenTopography DEM Downloader algorithm. After downloading the bathymetry data, a validity mask is created from the DEM. This mask identifies where the bathymetry data is valid. It checks if the elevation value is equal to 0, indicating areas of no data. These areas are presumed to represent water. The downloaded bathymetry data raster is then reprojected, clipped, and aligned with the DEM data raster. The following expression is used in the Raster Calculator to apply the merge. The expression used in RCMerge ensures that where bathymetry data is available, it is used, and where it's not available, the DEM data is retained. 

("A@1" * ("C@1" * ("C@1" <= -1 ) + -1 * ("C@1" > -1))) + (1 - "A@1") * "B@1"

A = Validity Mask
B = DEM data raster
C = Bathymetry data Raster

The SRTM15+ is very low resolution (aprox. 92m @ 15arcseconds). It does not capture complex seabed features. However, it does show general trends in seafloor depths.

#### 
## Development
Currently in the works:

(1) DSM to DTM Converter.

(2) Experimental Terrain Quality Enhancement Processing Options.

(3) QGIS Map Canvas Visualisations.

(4) Scripting for the automation of CRS identification based on user coordinates.

(5) Cubic / Lanczos

(6) Sea Level User Specification

(7) Burning Rivers and Lakes

