# This program is to sumarize predicted soil types in raster format by

# SLC polygons.

# Input: SLC ploygon Shapefile and soil type in raster format. The two inputs

# should have the same projection such as LCC with WGS84 datum

# Output: a csv table using the CMP table template
 

rm(list=ls())

library(raster)

library(rgdal)

library(sp)

library(plyr)

#library(stars)

library(ggplot2)

library(exactextractr)

library(dplyr)

library(data.table)

library(tidyr)

library(foreign)

library(spatial)

library(sf)

library(sfc)

library(terra)

library(spatialEco)

 


# Please select a work space or folder where both the inputs and outputs are stored

workspace.name=choose.dir(default = "", caption = "Select folder that contains watershed data")

wd=setwd(workspace.name)

getwd()

polygon.file.path <- getwd()


# The two files names are hard coded here although can be interactively selected.

GGSoils <- raster("GGSoilTypeTest250m.tif")

SLCv4VectorFile <- "SLCV4TestPolygonLCCWGS84"

 

slope_tif <- c("new_aligned_slope.tif" )

 

polygons <- st_read( "SLCV4TestPolygonLCCWGS84.shp")

polygons_df <- data.frame(polygons)

 

#SLCv4Polygon=read_sf(dsn=polygon.file.path,layer=SLCv4VectorFile)

SLCv4Polygon=readOGR(dsn=polygon.file.path,layer=SLCv4VectorFile)

names(SLCv4Polygon)

cmptable <- matrix(data = NA, ncol = 4,byrow = TRUE)

colnames(cmptable) <- c('SL','CMP','PCT','SoilID')

cmptable<-data.frame(cmptable)


#if (covariants@crs@projargs!=proj4string(boundary)){boundary=spTransform(boundary,covariants@crs)}

for (i in 1:nrow(coordinates(SLCv4Polygon))) {

  slc4polyID <- SLCv4Polygon[i, ]$POLY_ID

  GGSoils.in.a.polygon = crop(GGSoils,SLCv4Polygon[i,],snap = "out")

  boundary.raster=rasterize(SLCv4Polygon[i,], GGSoils.in.a.polygon)

  GGSoils.masked <- mask(GGSoils.in.a.polygon, mask=boundary.raster)

  GGSoilIDs <- unique(na.omit(GGSoils.masked[GGSoils.masked < 127]))

  GGSoils.masked <- na.omit(GGSoils.masked[GGSoils.masked < 127])

  PixelCount <- length(GGSoils.masked)

  pctvalues = double(length(GGSoilIDs))

 

  for (i in 1:length(GGSoilIDs)){

    PixelsOfASoil <- length(GGSoils.masked[GGSoils.masked == GGSoilIDs[i]])

    PercentOfCMP <- (PixelsOfASoil/PixelCount)*100

    pctvalues[i]<-PercentOfCMP

  }

  cmprank = rank(-pctvalues)

  for (j in 1:length(GGSoilIDs)){

    cmptable[nrow(cmptable)+1,]<- c(slc4polyID, cmprank[j], pctvalues[j], GGSoilIDs[j])

  }

}

 
new_cmptable <- cmptable[-1,]
nrow(polygons_df)

 
##output results to DBF and csv files

write.dbf(new_cmptable, "newTestCMP.dbf", row.names = FALSE)

write.csv(new_cmptable, "newTestCMP.csv", row.names = FALSE)
