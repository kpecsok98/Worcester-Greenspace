# Greenspace in Worcester
This tutorial was created by Kyle Pecsok. If you have any question please feel free to email me at kpecsok@clarku.edu.

### Things needed for this tutorial
- A Google Colab account, unless you have another coding software and know how to connect input data to the code, and place data created in the code to a new output folder.

- A Carto account, however even if you don't have a Carto account you can still produce the necessary shapefiles you just won't be able to create the same maps that I did.


## Introduction to the Tutorial
For this tutorial you will learn how to use the geopandas library in Python to create a few different maps within Carto that shows the prescence of greenspace within Worcester, and how the prescence of greenspace varies in Worcester neighborhoods that are deemed Environmental Justice neighborhoods with all criteria, Environmental Justice neighborhoods with some criteria and neighborhoods that are not deemed Environmental Justice neighborhoods. Keep reading to learn what an Environmental Justice neighborhood with some or all criteria are. 

Below are the following maps that will be created for this tutorial. If you wish to view these maps on Carto there is a link next to each image, which will send you to Carto to show an interactive version of that map. This is very helpful in allowing viewers to examine where in Worcester greenspace, Environmental Justice neighoborhoods are, and where Worcester greenspace is located within Environmental Justice neighborhoods. Using Carto is a great way to look interactively look at the locations of the shapefiles based on street and landmarks, for instance through this map I can zoom in and see which neighborhoods surrounding Clark Univeristy (my institution) considered Environmental Jusitce neighborhoods. You will notice that thought at in addition I have a map showing tree cover in Worcester. This was a map that I created using Worcester treecover data, however this data is not allowed for public use so I will not be sharing it, but if you are interested in seeing how I got the treecover data into this Carto Map, I will have a section explaining this. Perhaps if you want to add additional data to your map that I didn't include, it could potentially be helpful to review the treecover section.

While the maps I made were intended specifically for Worcester, you can do these maps for any town in Massachusetts with the data I provided. In the instructions of this tutorial I will explain how you can choose to do a different Massachusetts town. 


In this tutorial you will learn to do the following operations with the geopandas library:

**Project**: Switch the Coordinate Reference System (CRS) of a shapefile.

**Clip**: Create a new shapefile by cutting out the input shapefile to fit within the Clip feature shapfile.

**Select by Attribute**: Create a new shapefile by selecting certain criteria within a shapefile.

**Difference**: Create a new shapefile by removing parts of the input shapefile that fall within another shapefile. This is the equivalent to the Erase Operation in ArcMap. 

If you wish to see these maps, please check them out at these links.


### Objective 
Having greenspace and treecover, particularly in urban communities, can provide many benefits that grey infrastructure can't provide such as inhibiting the impacts of the Urban Heat Island Effect, absorbing pollutants, and reducing the risk of flooding and erosion (Hebbert and Jankovi, 2011; Burger et al., 2017). In addition they can provide mental and physical health benefits to residents who in live in areas with these spaces (Wolch et al., 2014; Sallis and Ganz, 2009). 

However studies show that spaces disproportionately lie within communities that are white and affluent (Boone at al., 2009; Wolch et al., 2014). Through this tutorial the objective is to see if it appears through visualization that this is the case in Worcester as well. In order to do this, the tutorial will lead the user to create layers that show the prescence of greenspace in non-Environmental Justice neighborhoods and Environmental Justice neighborhoods. Well what is an Environmental Justice neighborhood? An Environmental Justice neighborhood as directed by the Massachusetts Executive Office of Energy and and Environmental Afairs has to fulfill one of the following criteria at the Census Block Group (CBG) level:

1. Have a minority population that is >= 25%.

2. A 2010 median household income of less than $40,673.

3. Have >= 25% of households that are English isolated, meaning they don't speak English as their first language.

In addition to comparing greenspace distribution in Environmental Justice neigbhorhoods and non-Enviornmental Justice neighborhoods this tutorial will also show how to compare greenspace distribution in Environmental Justice neighborhoods that satisfy all three criteria, and Environmental Justice neighborhoods that satisfy only some criteria. Lastly, it is also important to note that these qualifications for Environmental Justice neighborhoods are only used for CBGs in Massachusetts.

### Data Needed for this Lab
1. Massachusetts Towns(): This data is available at this link and contains all the towns . Once the data is downloaded you should use the 

2. Environmental Justice CBG: This data is available at this link and contains all the Environmental Justice CBGs within Massachusetts. 

3. Greenspace (): The data is available at this link and represents all conservation lands and outdoor recreational facilities. 

4. Treecover (): While this data is not available for use, this data was produced by former Clark Phd student Arthur Elemes and represents the treecover in all of Worcester. 


## Tutorial
Please note that this tutorial is run on the assumption that the user is using Google Colab. So when connecting the input data and placing the output data, you will need to determine how to do this if you're not using Google Colab. If you're using Google Colab be sure to upload data to your Google Drive.

### Part 1 (Importing Libraries and Downloading Data)
First download all the libraries that will be needed for this lab
```Python
!pip install geopandas
!apt-get install -y libspatialindex-dev
!pip install rtree

import geopandas as gpd
import pandas as pd
import rtree # needed for doing the geometric operations with geopandas 
from shapely.geometry import Point, Polygon, MultiPolygon  # for manipulating text data into geospatial shapes
from shapely import wkt  # stands for "well known text," allows for interchange across GIS programs
```


Next you will want to organize the data. I recommend creating a folder called Worcester_EJ_Greenspace as I did, and then create an input folder within the Worcester_EJ_GreenSpace folder. If you're using Colab, before inputting the data you need to write the following code to connect Colab to your Drive.

```Python
from google.colab import drive
drive.mount('/content/gdrive') # connects Colab to your Google Drive
root_path = 'gdrive/My Drive/Worcester_EJ_GreenSpace/' # set root path to folder where you uploaded the data
```


Once Colab is connected to your Drive type in the following code. Again please note that if you are NOT using Colab you should NOT use this exact code to connect to your input data. Also, for anyone, don't attempt to download the tree_cover data, I am only including it to show you how I used this specific data to include in a Carto map. If there is data that I did not provide that you would like to use feel free to import it.

```Python
# Import shapefiles
environmentaljustice = gpd.read_file(root_path+'input/EJ_POLY.shp') # Shapefile of Massachusetts Environmental Justice CBGs
greenspace = gpd.read_file(root_path+'input/OPENSPACE_POLY.shp') # Shapefile of Massachusetts open space areas 
towns = gpd.read_file(root_path+'input/CENSUS2010TOWNS_POLY.shp') # Shapefile of Massachusetts towns
tree_cover = gpd.read_file(root_path+'input/UTCWooManualEdits_20150907_AElmes.shp') # You will not import this shapefile for the tutorial
```

### Part 2 (Checking Shapefile Projection and Reprojecting Data)
For data to be used in Cato they all need to have the WGS 1984 Coordinate Reference System (CRS). However, we don't know what the coordinate reference system of our data is just from simply downloading it. Luckily, there is a way to check the CRS. First let's check the CRS of ```environmentaljustice```. To do this type the following code.

```Python
# Check the Coordinate Reference System of environmentaljustice
environmentaljustice.crs
```

You're output should look like the following ______ Clearly this is shapefile is not set to the WGS 1984 CRS. Let's check the other shapefiles.

```Python
# Check the Coordinate Reference System of greenspace
greenspace.crs
```

```Python
# Check the Coordinate Reference System of towns
towns.crs
```

For this treecover data I'm only doing this because I have the data. If you decide to add any of your own data just type it out the same way to check its CRS.

```Python
# Check the Coordinate Reference System of tree_cover
tree_cover.crs
```

You should see that none of the data has the CRS we need to upload to Carto. This means we will need to project it to the WGS 1984 CRS. First we will reproject the  ```environmentaljustice``` by typing the following code.

```Python
# Project environmentaljustice to the WGS 1984 CRS
environmentaljustice = environmentaljustice.to_crs("EPSG:4326")
```
The EPSG code 4326 is the specific code for the WGS 1984 CRS. So in order to project the other data to this CRS, just do the same thing as above, except now replace ```environmentaljustice``` with ```greenspace``` and ```towns```.

```Python
# Project greenspace to the WGS 1984 CRS
greenspace = greenspace.to_crs("EPSG:4326")
```

```Python
# Project towns to the WGS 1984 CRS
towns = towns.to_crs("EPSG:4326")
```

I also projected the tree_cover data. Again this is not data that you as the user of this tutorial use, but if you have any extra data feel free to project that if it needs to be.

```Python
# Project tree_cover to the WGS 1984 CRS
tree_cover = tree_cover.to_crs("EPSG:4326")
```

Now let's check the CRS of one of the files we initially downloaded. Let's check the CRS of  ```environmentaljustice```.

```Python
# Check the new Coordinate Reference System of environmentaljustice
environmentaljustice.crs
```

You should now get this output. Feel free to check the other shapefiles that your projected if you would like, they all should have that CRS now.


### Part 3 (Create new shapefiles)

Now that we have all our data in the correct CRS, let's start creating new shapefiles. First we are going to perform a select by attribute in our towns data, where we will select WORCESTER. In the code below I am selecting within the towns attribute file, an item in the TOWN column that are Worcester, which in other words means that of all the Massachusetts towns I'm only selecting Worcester for my new shapefile. While ```Worcester``` is not yet a shapefile, I will show later on in this section how to convert it to a shapefile. Keep in mind if you would like you can do another Massachusetts town, just be sure you type it in all caps like WORCESTER is below.


```Python
#From Massachusetts towns file select by attribute to get Worcester
Worcester = towns[towns['TOWN']=="WORCESTER"] #Within TOWN column of attribute table select WORCESTER
```
To see and get familiar with this Worcester data type the following code.

```Python
# Check data of 1 row of Worcester 
Worcester.sample(1)
```

You should get an output that looks like this. 

Now let's do the same thing, but type it out as this.

```Python
# Check data of 2 rows of Worcester 
Worcester.sample(2)
```

Did you get a message like this?

If you you did then that is actually a good sign. Since we only selected one row when we did Select by Attribute, ```Worcester``` only has 1 row which means that we can't sample two rows of this data. So if we have an error by typing ```Worcester.sample(2)```, it's a good sign that you did the Select by Attribute correctly. Another way to check to see if you created  ```Worcester``` type out this code.

```Python
# Create a map of Worcester
Worcester.plot(column='TOWN', color='grey', figsize=(16,8));
```

Does your ouput look like this? 

If it does great! That polygon is of the city of Worcester. If your output doesn't look like that, go back and make sure that you did all of the prior steps correctly. For the rest of the tutorial I'm not going to do this again, however if you want to double check to see if your select by attribute, clip, or difference was performed correctly in the following steps, just follow the same format as above to see what the polygon of your new file looks like. When doing this just be sure that in ```column=``` you enter, you enter the name of a column that is actually in the data's attribute table. 

Next we want to take the Environmental Justice CBG shapefile and do a Select by Attribute again. Just like with the towns data we will select items in the TOWNS column that is Worcester, which means we will select Environmental Justice CBGs that are in Worcester to create a new shapefile these types of CBGs that are solely within Worcester.

```Python
#From Environmental Justice Block group file select by attribute to get Worcester
Worcester_EJ = environmentaljustice[environmentaljustice['TOWN']== "WORCESTER"] #Within TOWN column of attribute table select WORCESTER
```

Even though we have a variable now that contains Environmental Justice CBGs in Worcester, there's still more we need to do. We don't just want to compare greenspace in Enivronmental Justice CBGs and Non-Environmental CBGs, we also want to see how greenspace in Worcester compares in Environmental Justice CBGs that fulfill all three criteria, and Environmental Justice CBGs that fulfill some criteria. If you want to see what the criteria are, go back to the Objective section of this tutorial where I list off the three Environmental Justice criteria. So first let's work to create a shapefile that is of Environmental Justice CBGs that fulfill all three criteria, we will do this again through a Select by Attribute. In this code below we simply just select Environmental CBGs within Worcester that satisfy all three criteria, which means we only select those CBGs that have a ```CRIT_CNT``` (short for Criteria Count) that is equal to three.

```Python
#From Worcester Environmental Justiec Block Group file select by attribute to select Block Groups with all three criteria.
Worcester_EJ_all = Worcester_EJ[Worcester_EJ['CRIT_CNT']==3] #Within CRIT_CNT column of attribute select block groups with a value of 3.
```

Now we want to work towards creating a shapefile of Environmental Justice CBGs in Worcester that only fulfill some criteria. To do thiis we will type pretty similar code as last time except we will want ```CRIT_CNT``` to be values of less than 3, in other words we are selecting by attribute for Environmental Justice CBGs that fulfill less than three criteria.

```Python
# From Worcester Environmental Justiec Block Group file select by attribute to select Block Groups with all three criteria.
Worcester_EJ_some = Worcester_EJ[Worcester_EJ['CRIT_CNT']<3] # Within CRIT_CNT column of attribute select block groups with a value less than 3.
```

Next we want to create geodataframes that contain data of greenspace exclusively in Worcester. We can do this with a Clip. First let's clip ```greenspace``` within Worcester by typing the following code. 

```Python
#Clip greenspace within Worcester
Worcester_greenspace = gpd.clip(greenspace, Worcester)
```

Now we have a geodataframe of greenspace within the Worcester city limits. In our final maps though we want to see where are the greensapces that fall within Worcester EJ CBGs that fulfill some or all criteria. Next let's create a geodataframe of greenspace that fall within Worcester EJ CBGs that fulfill all criteria.

```Python
#Clip the Worcester greenspace file within the Worcester Environmental Justice block groups with all criteria file
Worcester_EJ_all_greenspace = gpd.clip(Worcester_greenspace, Worcester_EJ_all)
```

Now we'll create a geodataframe of greenspaces within Worcester EJ CBGs that only fulfill some criteria.

```Python
#Clip the Worcester greenspace file within the Worcester Environmental Justice block groups with some criteria file
Worcester_EJ_some_greenspace = gpd.clip(Worcester_greenspace, Worcester_EJ_some)
```

Lastly, we want to create a geodatagrame that contains of Worcester greenspace that is not within an EJ CBG, regardless of how many criteria it has. To do this type the following code.

```Python
# Erase Worcester greenspace within areas that Worcester Environmental Justice block groups are in
Worcester_greenspace_NonEJ = gpd.overlay(Worcester_greenspace, Worcester_EJ, how = 'difference')
```

We are now done with typing geodataframes. When I initally created this tutorial I was hopeful that I could clip the treecover data within the EJ CBGs with all and some criteria, but unfortunately by Google Colab crashed multiple times when trying to do so :(. So instead, I just ended up showing the projected tree_cover data in a Carto data, however the code below shows how I would have clipped the tree_cover data had it worked. 

```Python
#Code to clip treecover within the Worcester Environmental Justice block groups file
'''
Worcester_EJ_all_treecover = gdp.clip(Worcester_treecover, Worcester_EJ_all)
'''
```

```Python
#Code to clip Worcester treecover within the Worcester Environmental Justice block groups with some criteria file
'''
Worcester_EJ_some_treecover = gdp.clip(Worcester_treecover, Worcester_EJ_some)
'''
```

```Python
#Code to erase Worcester treecover data that are within the Environmeantl Justice block groups.
'''
Worcester_treecover_NonEJ = gpd.overlay(Worcester_greenspace, Worcester_EJ, how = 'difference')
'''
```
