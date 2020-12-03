# Worcester-Greenspace


# Introduction to the Tutorial
For this tutorial you will learn how to use the geopandas library in Python to create a few different maps within Carto that shows the prescence of greenspace within Worcester, and how the prescence of greenspace varies in Worcester neighborhoods that are deemed Environmental Justice neighborhoods and ones that are not. Below are the following maps that will be created for this tutorial. You will notice that I have an additional maps showing tree cover in Worcester. This was a map that I created using Worcester treecover data, however this data is not allowed for public use so I will not be sharing it, but if you are interested in seeing how I got the treecover data into this Carto Map, I will have a section explaining this. Perhaps if you want to add additional data to your map that I didn't include, it could potentially be helpful to review the treecover section.

While the maps I made were intended specifically for Worcester, you can do these maps for any town in Massachusetts with the data I provided. In the instructions of this tutorial I will explain how you can choose to do a different Massachusetts town. 


In this tutorial you will learn to do the following operations with the geopandas library:

Project: Switch the Coordinate Reference System (CRS) of a shapefile.

Clip: Create a new shapefile by cutting out the input shapefile to fit within the Clip feature shapfile.

Select by Attribute: Create a new shapefile by selecting certain criteria within a shapefile.

Difference: Create a new shapefile by removing parts of the input shapefile that fall within another shapefile. This is the equivalent to the Erase Operation in ArcMap. 

If you wish to see these maps, please check them out at these links.


# Objective 
Having greenspace and treecover, particularly in urban communities, can provide many benefits that grey infrastructure can't provide such as inhibiting the impacts of the Urban Heat Island Effect, absorbing pollutants, and reducing the risk of flooding and erosion (Hebbert and Jankovi, 2011; Burger et al., 2017). In addition they can provide mental and physical health benefits to residents who in live in areas with these spaces (Wolch et al., 2014; Sallis and Ganz, 2009). 

However studies show that spaces disproportionately lie within communities that are white and affluent (Boone at al., 2009; Wolch et al., 2014). Through this tutorial the objective is to see if it appears through visualization that this is the case in Worcester as well. In order to do this, the tutorial will lead the user to create layers that show the prescence of greenspace in non-Environmental Justice neighborhoods and Environmental Justice neighborhoods. Well what is an Environmental Justice neighborhood? An Environmental Justice neighborhood as directed by the Massachusetts Executive Office of Energy and and Environmental Afairs has to fulfill one of the following criteria at the Census Block Group (CBG) level:

1. Have a minority population that is >= 25%.

2. A 2010 median household income of less than $40,673.

3. Have >= 25% of households that are English isolated, meaning they don't speak English as their first language.

In addition to comparing greenspace distribution in Environmental Justice neigbhorhoods and non-Enviornmental Justice neighborhoods this tutorial will also show how to compare greenspace distribution in Environmental Justice neighborhoods that satisfy all three criteria, and Environmental Justice neighborhoods that satisfy only some criteria. Lastly, it is also important to note that these qualifications for Environmental Justice neighborhoods are only used for CBGs in Massachusetts.
