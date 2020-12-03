# Greenspace in Worcester
This tutorial was created by Kyle Pecsok. If you have any question please feel free to email me at kpecsok@clarku.edu.

### Things needed for this tutorial
- A Google Colab account, unless you have another coding software and know how to connect input data to the code, and place data created in the code to a new output folder.

- A Carto account, however even if you don't have a Carto account you can still produce the necessary shapefiles you just won't be able to create the same maps that I did.


## Introduction to the Tutorial
For this tutorial you will learn how to use the geopandas library in Python to create a few different maps within Carto that shows the prescence of greenspace within Worcester, and how the prescence of greenspace varies in Worcester neighborhoods that are deemed Environmental Justice neighborhoods with all criteria, Environmental Justice neighborhoods with some criteria and neighborhoods that are not deemed Environmental Justice neighborhoods. Keep reading to learn what an Environmental Justice neighborhood with some or all criteria are. 

Below are the following maps that will be created for this tutorial. If you wish to view these maps on Carto there is a link next to each image, which will send you to Carto to show an interactive version of that map. This is very helpful in allowing viewers to examine where in Worcester greenspace, Environmental Justice neighoborhoods are, and where Worcester greenspace is located within Environmental Justice neighborhoods. You will notice that I have an additional maps showing tree cover in Worcester. This was a map that I created using Worcester treecover data, however this data is not allowed for public use so I will not be sharing it, but if you are interested in seeing how I got the treecover data into this Carto Map, I will have a section explaining this. Perhaps if you want to add additional data to your map that I didn't include, it could potentially be helpful to review the treecover section.

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

### Part 1 (Importing Libraries and Downloading Data
First download all the libraries that will be needed for this lab



Next you will want to organize the data I recommend creating a folder called Worcester_greenspace, or whatever name you would like to give the folder, and then create an input folder the initial folder created. You'll notice that my folder is called IDCE30274_FinalProject, but I recommend not doing this, unless you're doing this tutorial or your IDCE 30274 final project. If you're using Colab before inputting the data you need to write the following code to connect to your Drive.


Once Colab is connected to your Drive type in the following code. Again please note that if you are NOT using Colab you should NOT use this exact code to connect to your input data and even if you are using Colab if the folder you created is not named 'IDCE30274_Final' please replece that file name. 






