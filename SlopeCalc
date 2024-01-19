 
# This program is to sumarize predicted soil types in raster format by

# SLC polygons.

# Input: SLC ploygon Shapefile and soil type in raster format. The two inputs

# should have the same projection such as LCC with WGS84 datum

# Output: a csv table using the CMP table template


library(raster)

library(rgdal)

library(sp)

library(plyr)

library(dplyr)

library(ggplot2)

 

# Please select a work space or folder where both the inputs and outputs are stored

workspace.name = choose.dir(default = "", caption = "Select folder that contains watershed data")

wd = setwd(workspace.name)

getwd()

polygon.file.path <- getwd()

 

GGSoils <- raster("GGSoilTypeTest250m.tif")

 

slope <- raster("slope_testdata_250m_lcc_wgs84.tif")

 

cmptab <- read.csv("test_slc_cmp.csv", header = TRUE)

cmptab <- data.frame(cmptab)

 

# Add new column names for a to be summarized attribute e.g. slope

cmptab <- cmptab %>% mutate(slopeQ1 = NA, slopeMedian = NA, slopeQ3 = NA, slopeMean = NA, SLOPECLASS = NA)

 

#WORKS

SLCv4Polygon = shapefile("slc_v4_testdata_lcc_wgs84.shp")

 

newcmptab <- cmptab[0, ]

 

for (i in 1:nrow(coordinates(SLCv4Polygon))) {

  slc4polyID <- SLCv4Polygon[i, ]$POLY_ID

  GGSoils.in.a.polygon = crop(GGSoils,SLCv4Polygon[i,],snap = "out")

  slope.in.a.polygon = crop(slope,SLCv4Polygon[i,],snap = "out")

  boundary.raster = rasterize(SLCv4Polygon[i,], GGSoils.in.a.polygon)

 

  GGSoils.masked <- mask(GGSoils.in.a.polygon, mask = boundary.raster)

  slope.masked <- mask(slope.in.a.polygon, mask = boundary.raster)

 

 

  GGSoils.masked <- GGSoils.masked[GGSoils.masked>0]

  GGSoils.masked <- na.omit(GGSoils.masked[GGSoils.masked < 127])

  GGSoilIDs <- unique(na.omit(GGSoils.masked[GGSoils.masked < 127]))

  slope.masked <- na.omit(slope.masked[slope.masked > 0])

 

  # Loop through the soil types within a polygon area

  for (j in 1:length(GGSoilIDs)){

    slopebyGGS <- slope.masked[GGSoils.masked == GGSoilIDs[j]]

    temptab <- cmptab %>% filter(POLY_ID == slc4polyID, CMP == GGSoilIDs[j])

    temptab <- as.data.frame(temptab)

    summary<-summary(slopebyGGS)

    temptab$slopeQ1[temptab$SoilID == GGSoilIDs[j]] <- as.numeric(zonal.stats['1st Qu.'])

    temptab$slopeMedian[temptab$SoilID == GGSoilIDs[j]] <- as.numeric(zonal.stats['Median'])

    temptab$slopeQ3[temptab$SoilID == GGSoilIDs[j]] <- as.numeric(zonal.stats['3rd Qu.'])

    temptab$slopeMean[temptab$SoilID == GGSoilIDs[j]] <- as.numeric(zonal.stats['Mean'])

    newcmptab <- rbind(newcmptab, temptab)

   

  }

}

 

newcmptab

#create new col SLOPE which letter grades slope % based on SLC cmp table

 

for (k in 1:nrow(newcmptab)) {

 

  if (newcmptab$slopeMean[k] <= 3)

  {

  newcmptab$SLOPE_CLASS[k] <- 'A' 

  }

 

  else if (newcmptab$slopeMean[k] <= 9) {

  newcmptab$SLOPE_CLASS[k] <- 'B' 

  }

 

  else if (newcmptab$slopeMean[k] <= 15){

  newcmptab$SLOPE_CLASS[k] <- 'C' 

  }

 

  else if (newcmptab$slopeMean[k] <= 30){

  newcmptab$SLOPE_CLASS[k] <- 'D' 

  }

 

  else if (newcmptab$slopeMean[k] <= 60){

  newcmptab$SLOPE_CLASS[k] <- 'E' 

  }

  else if (newcmptab$slopeMean[k] > 60){

  newcmptab$SLOPE_CLASS[k] <- 'F' 

  }

  else {

  newcmptab$SLOPE_CLASS[k] <- 'NA' 

  }

 

}

newcmptab

 

#create csv file with results

write.csv(df, "newCMP2withslope.csv", row.names=FALSE)
