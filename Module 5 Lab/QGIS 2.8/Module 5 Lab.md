# GST 103: Data Acquisition and Management 
## Lab 5 - Raster Data Structure
### Objective – Work with the Raster Data Model

Document Version: 4/26/2015

**FOSS4G Lab Author:**
Kurt Menke, GISP
Bird's Eye View GIS

**Original Lab Content Author:**
Richard Smith, Ph.D., GISP
Texas A&M University - Corpus Christi

---

The development of the original document was funded by the Department of Labor (DOL) Trade Adjustment Assistance Community College and Career Training (TAACCCT) Grant No.  TC-22525-11-60-A-48; The National Information Security, Geospatial Technologies Consortium (NISGTC) is an entity of Collin College of Texas, Bellevue College of Washington, Bunker Hill Community College of Massachusetts, Del Mar College of Texas, Moraine Valley Community College of Illinois, Rio Salado College of Arizona, and Salt Lake Community College of Utah.  This work is licensed under the Creative Commons Attribution 3.0 Unported License.  To view a copy of this license, visit http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative Commons, 444 Castro Street, Suite 900, Mountain View, California, 94041, USA.

This document continues to be modified and improved by generous public contributions.

---

### 1. Introduction

Raster data is a data model in which the data is presented in a grid. Each grid cell contains one data value or attribute. For example, a digital elevation model (DEM) has cell values that represent the elevation. One important characteristic of raster data is the resolution. Raster resolution is a measure of the cell dimensions, which means the area that each cell covers in the real world. For example, a satellite image may have a resolution of 30 meters, which means that each cell covers 30 square meters in the real world. You can use raster data simply as cartographic backdrops, as datasets for digitizing, or for analysis.

This lab includes the following tasks:

+ Task 1 Merging and Clipping Raster Data
+ Task 2 Raster Pyramids

### 2. Objective: Work with the Raster Data Model

This lab focuses on working with raster data within QGIS.

### Task 1 Merging and Clipping Raster Data

Raster data are often provided in tiles, such as USGS Quadrangles. In such cases, it is necessary to merge the raster tiles together to form a seamless raster covering the study area. 

2. Open QGIS Desktop and add the four DEM raster datasets (35106-A4.dem, 35106-A5.dem, 35106-B4.dem and 35106-B5.dem) (shown in figure below).

![Four DEMs Covering the Study Area](figures/Four_DEMs_Covering_the_Study_Area.png "Four DEMs Covering the Study Area")

Each of these has cell values representing the elevation above sea level. Each is styled with the values stretched across a black to white color ramp. Since each dataset has different minimum and maximum cell values, the boundaries between datasets is obvious.

4. Save your project as Lab_5.qgs.
5. Double-click on the 35106-B5.dem layer to open the Layer Properties.
6. Click on the Metadata tab.

In the Properties window you will find information about the file format (Driver), cell statistics (Band 1), Dimensions, Origin, Pixel size (10 meters), the No Data value and the Data Type. 

Raster datasets are always rectangular. If the data content does not fill the rectangular area, the extra cells will be assigned a value that signifies that there is no data there. Here the No Data value is -32767.

9. Close the Layer Properties.
10. Turn on the Processing toolbox by clicking on Processing | Toolbox.
11. Select the Advanced interface from the dropdown selection box at the bottom of the Processing Toolbox panel.
11. From the Toolbox choose SAGA | Grid-Tools | Mosaick raster layers (shown in figure below).
12. Fill out the Mosaic raster dialog with the following parameters:

	a. Input Grids = Click the ellipsis button ![ellipsis button](figures/ellipsis_button.png "ellipsis button") in the upper right corner and select all four dem rasters. Click OK.

	b. Preferred data storage type = [6] 4 byte signed integer.

	c. Interpolation = [1] Bilinear Interpolation

	d. Overlapping cells = [1] last value in order of grid list

	e. Mosaicked grid = Save to file: Lab 5 Data\\Mosaick.tif

	f. When parameters match the figure below, click Run.

![Mosaick Raster Layers](figures/Mosaick_raster_layers.png "Mosaick Raster Layers")

12. Turn off the input DEMs in the Layers panel. You now have a seamless raster (shown in figure below).

![Mosaicked DEM](figures/Mosaicked_DEM.png "Mosaicked DEM")

13. Now you will clip the mosaicked dem to the project study area.
14. Add the StudyArea.shp shapefile to QGIS Desktop.
15. From the menu bar choose Raster | Extraction | Clipper.
16. Set the following parameters for the Clipper (shown in figure below):

	a. Input file (raster) = Mosiack

	b. Output file = Lab 5 Data\\StudyArea.tif

	c. Clipping Mode = Mask Layer

	d. Mask layer = StudyArea

	e. Click OK.

	f. Click Close when done.

![Clipping a Raster](figures/Clipping_a_Raster.png "Clipping a Raster")

16. Turn off the visibility for the StudyArea and Mosaick layers to see the clipped raster.
17. Save your project.

This is a common workflow to get raster data set up for analysis.

### Task 2 Raster Pyramids

Pyramids are lower resolution versions of a raster dataset that are more suitable for display on a monitor. Without pyramids, the computer will attempt to render each and every pixel in a raster dataset, whether the computer monitor can display all the detail or not. Having pyramids greatly decreases the time it takes to render a raster on screen.

1. Open Lab_5.qgs in QGIS Desktop.
2. Open the Layer Properties for the Mosaick raster layer.
3. Click on the Pyramids tab. Currently this raster has no pyramids. The available resolutions are listed on the right side (shown in figure below).

![Layer Properties Pyramids](figures/Layer_Properties_Pyramids.png "Layer Properties Pyramids")

Pyramids can be embedded within the raster file, or built externally. It is safer to build them externally as this does not alter the original dataset. The external pyramid file can always be deleted if it does not have the desired results.

5. Select all 6 resolutions: 1164 x 1408 to 36 x 44.
6. For the Overview format, select External.
7. Set Resampling method to Cubic.

Generally nearest neighbor technique is most suitable for discrete rasters since it will not change the values of the cells. The average, gauss, and cubic techniques are more suitable for continuous rasters such as this DEM. They will cause some smoothing of the data and may result in some values that are beyond the original range.

8. Click Build Pyramids.
9. Click OK to close the Layer Properties.
10. Zoom in and out on the raster to see how quickly the raster renders on the screen.

*Note*: This dataset is small enough that you may not notice an improvement in drawing speed. However, it can be quite dramatic for large rasters over 100 Mb in size.

7. Re-open the layer properties for the Mosaick raster layer.
8. Switch to the Metadata tab.
9. In the Properties section, scroll to the Dimensions section. Under Dimensions you will see multiple dimension entries indicating that the pyramids resolutions were built.
8. Open a file browser (for example: Windows explorer or Finder) and navigate to the Lab 5 Data folder. You will see a Merge.tif.ovr file. This is the file containing the pyramids.

### 3. Conclusion
In this lab you focused on preparing raster data so that it seamlessly covers a study area. You also learned how to build pyramid files for a raster dataset.

### 4. Discussion Questions

1. What is a raster dataset?
2. Compare and contrast raster and vector data models.
3. Why might you use raster data? Give two examples.

### 5. Challenge Assignment (optional)

Using the National Map (http://viewer.nationalmap.gov/viewer/) download DEMs for an area interesting to you. 

1. Select an area of interest using the Download by Bounding Box tool ![Bounding Box tool](figures/Bounding_Box_tool.png "Bounding Box tool") . With this tool, drag a box around an area roughly the size of a large county or several small counties.
2. Click the Download Data ![Download Data button](figures/Download_Data_button.png "Download Data button")  button. 
3. From the USGS Available Data for download select Elevation.

![USGS Available Data for Download](figures/USGS_Available_Data_for_download.png "USGS Available Data for Download") 

4. All the available DEMs will be displayed.
5. Select the 1/9 arc-second datasets covering your area.

![USGS Available Data for download 2](figures/USGS_Available_Data_for_download_2.png "USGS Available Data for download 2") 

6. Click Next

7. Click the ![Checkout](figures/Checkout.png "Checkout")  button.

8. Enter your email address.

9. Click the ![Place Order](figures/Place_Order.png "Place Order")  button.

10. You will be notified via email when the data are available. You will be provided with download links for each dataset. Download and unzip the data.
11. Merge the rasters together and build pyramids for the merged dem.