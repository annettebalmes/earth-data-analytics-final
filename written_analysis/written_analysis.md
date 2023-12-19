# earth-data-analytics-final
Grassland Management Under Climate Change 
- Evaluation of Sorghastrum nutans likelihood in several regions

## Introduction
Sorghastrum nutans, commonly known as Indiangrass, is a warm-season perennial grass native to North America, according to the Natural Resources Conservation Service. It is often found in prairies, savannas, and open woodlands, and is found from the east coast to the Rocky Mountains, Arizona, Wyoming, and Utah (Michigan Natural Features Inventory, n.d.).

Fast Facts
* It grows from 3 to 8 feet tall
*Indiangrass has a medium tolerance
to salinity and drought
*It usually
occurs in non-wetlands, but may occur in wetlands
* It's roots can grow to a depth of 2 meters.

## Data Choice
If we analyze different types of data like pH, slope, and precipitation inputted into a fuzzly logic model can help us find the areas most suitable for Indiangrass to occur (Brakie, 2017).

We can find Indiangrass by looking at it's soecific characteristics:
* pH range of 4.8 to 8.0
* middle and lower slopes from 10 - 40%
* annual precipitation of 28 to 114 cm

We can aquire that data from:
* pH: soil depth 100-200 cm mean pH data from POLARIS
* slope: APPEEARS API for elevation data from NASA Earthdata
* annual precipitation: MACAv2 dataset, accessible from Climate Toolbox.

## Locations to Test out my Model

According to the USDA, Indiangrass occurs as a dominant or subdominant in the following
classifications:
     
* Composition, classification and species response patterns of remnant
   tallgrass prairies in Texas.
* Classification of native vegetation at the Woodworth Station, North
   Dakota.
(U.S.D.A. n.d.)

With access to the USFS National Grassland Units, I found two locations in these areas where I knew Indiangrass was very likely to occur.
1. Lyndon B. Johnson National Grassland, Texas, USA.
2. Sheynne National Grassland, North Dakota, USA.

## More on Data:

### Polaris
POLARIS provides a spatially continuous, internally consistent, quantitative prediction of soil series. It is a 30-meter probabilistic soil series map of the contiguous United States. This database has the potential to improve the modeling of biogeochemical, water, and energy cycles in environmental models; enhance availability of data for precision agriculture; and assist hydrologic monitoring and forecasting to ensure food and water security, or in our case, to model Indiangrass (Chaney, 2016).

As we know that Indiangrass grows roots down to 2 meters, I acquired the 100-200cm mean pH data as it is more likely that the levels at the deeper roots could be prevelant.

### Appears API and NASA Elevation Data
AppEEARS stands for Application for Extracting and Exploring Analysis Ready Samples to assist users to subset geospatial datasets using spatial, temporal, and band/layer parameters. In this case we used the API to download NASA Shuttle Radar Topography Mission Global 3 arc second DEM data. The digital elevation model launched February 11, 2000 and ﬂew for 11 days and is the highest resolution elevation model available today (NASA, n.d.).

The API is not very stable, and I had to create code to cover multiple situations, including if the API is unaccessible. In addition, I have manually downloaded the data that I used in this project and used that in my final results.

### MACAv2 through Climate Toolbox
Multivariate Adaptive Constructed Analogs (MACA) is a statistical method for downscaling GCM data from its coarse format to a higher spatial resolution for the Continental United States (CONUS). The MACA downscaling approach takes 20 GCMs from CMIP5 and downscales them to 4km or 6km resolution data. The resolution of MACA v2 Metdata is 4km (MACA, n.d.).

The data variables include temperature, precipitation, humidity, downward shortwave solar radiation, and eastward and northward wind. Data are also provided for historical time periods from 1950-2005. I have downloaded monthly aggregated data from 1950-2005 for each location through the Climate Toolbox from the Northwest Knowledge Network(NKN) (NKN Data Catalogs for MACA Products, n.d.).

### USFS National Grassland Units
The Forest Service currently administers 20 National Grasslands consisting of 3,842,278 acres of federal land and the 20,000 acre Midewin National Tallgrass Prairie. National Grasslands are located in 13 states. The majority of the acreage (3,161,771 acres, 82%) of the total National Grassland area is in the Great Plains states of Colorado, North Dakota, South Dakota, Kansas, Oklahoma, Texas and Wyoming. Seventeen of the National Grasslands are located on the Great Plains (National Grasslands, n.d.). GIS layers of area boundaries are available for download [here](https://data.fs.usda.gov/geodata/edw/edw_resources/shp/S_USA.NationalGrassland.zip).

As I worked with the data, I needed to transform the data from geographic units to a universal trasverse mercator projection. Interestingly enough, both of my chosen locations ended up being in the same EPSG code: 32614, if someone wanted to reproduce my work, they would need to alter the code to allow different zones.

###  How to Build the Model
Create a habitat suitability model- For each grassland:

1. Download model variables as raster layers covering your study area envelope, including:
        * At least one soil variable from the POLARIS dataset
        * Elevation from the SRTM (available from the APPEEARS API)
        * At least one climate variable from the MACAv2 dataset, accessible from Climate Toolbox. *Undergraduate students may download a single year and scenario; Graduate students should download at least two)

2. Calculate at least one derived topographic variable (slope or aspect) to use in your model. You probably will wish to use the xarray-spatial library, which is available in the latest earth-analytics-python environment (but will need to be installed/updated if you are working on your own machine). Note that calculated slope may not be correct if you are using a CRS with units of degrees; you should re-project into a projected coordinate system with units of meters, such as the appropriate UTM Zone.
3. Harmonize your data - make sure that the grids for each of your layers match up. Check out the ds.rio.reproject_match() method from rioxarray.

4. Train a fuzzy logic model
   * Research S. nutans, and find out what optimal values are for each variable you are using (e.g. soil pH, slope, and current climatological annual precipitation).
   * For each digital number in each raster, assign a value from 0 to 1 for how close that grid square is to the optimum range (1=optimal, 0=incompatible).
   * Combine your layers by multiplying them together. This will give you a single suitability number for each square.

### Model: Lyndon B. Johnson National Grassland

<iframe src="/assets/multispectral/chloropleth-seattle-2019-present-max.html"
    sandbox="allow-same-origin allow-scripts"
    width="700"
    height="700"
    scrolling="no"
    seamless="seamless"
    frameborder="0">
</iframe>

## Data Citations
Brakie, M. 2017. Plant Guide for Indiangrass (Sorghastrum nutans). USDA-Natural Resources Conservation Service, East
Texas Plant Materials Center. Nacogdoches, TX 7596

Chaney, N. W., E. F. Wood, A. B. McBratney, J. W. Hempel, T. W. Nauman, C. W. Brungard, and N. P. Odgers. “POLARIS: A 30-meter probabilistic soil series map of the contiguous United States.” Geoderma 274 (July 15, 2016): 54–67. https://doi.org/10.1016/j.geoderma.2016.03.025.

Maca. Climatology Lab. (n.d.). https://www.climatologylab.org/maca.html 

Michigan Natural Features Inventory. Hillside Prairie - Michigan Natural Features Inventory. (n.d.). https://mnfi.anr.msu.edu/communities/description/10709/hillside-prairie

National Grasslands. US Forest Service. (n.d.). https://www.fs.usda.gov/managing-land/national-forests-grasslands/national-grasslands 

NKN Data Catalogs for MACA Products. Maca statistical downscaling method. (n.d.). https://climate.northwestknowledge.net/MACA/data_catalogs.php 

NASA. (n.d.). Aρρeears. NASA. https://appeears.earthdatacloud.nasa.gov/ 

U.S.D.A. (n.d.). Sorghastrum nutans. Sorghastrum Nutans. https://www.nrcs.usda.gov/plantmaterials/etpmcpg13196.pdf

