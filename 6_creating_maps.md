---
title: "Creating maps"
teaching: 120
exercises: 4
output: html_document

---



:::::::::::::::::::::::::::::::::::::: questions 

- How do we create maps using R?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Learn how to create static maps with ggplot2
- Learn how to create interactive maps with mapview
- Learn how to plot iNaturalist observations on a map

::::::::::::::::::::::::::::::::::::::::::::::::


```r
library(sf)
library(dplyr)
library(here)
library(readr)
library(ggplot2)
library(mapview)
```

## Geographic data

There are various file formats for geographic data. Shape files for GIS applications, KML for Google maps, geojson for web applications.

You can get boundaries for countries, states, cities, etc from various sources. I googled "Los Angeles county boundary shape" which had a link to "County Boundary | City of Los Angeles Hub - LA GeoHub" https://geohub.lacity.org/datasets/10f1e37c065347e693cf4e8ee753c09b  I downloaded the shapefile for LA county.

You can also create your boundaries using GIS applications or GIS web applications.

##  Mapping iNaturalist data

iNaturalist data includes latitude and longitude, which means we can put the observations in a map. There are several packages to create maps. We will use ggplot and mapview packages.

First, read data from the cleaned iNaturalist observation file.



```r
path = here('data', 'cleaned', 'observations.csv')
inat <- read_csv(path)
```

```error
Error: '/home/runner/work/CNC-coding-workshop/CNC-coding-workshop/site/built/data/cleaned/observations.csv' does not exist.
```

See all the column names. "latitude" and "longitude"  are the column names we need.


```r
names(inat)
```

```error
Error in eval(expr, envir, enclos): object 'inat' not found
```


We use the sf package to add geographic data to our dataframe. `st_as_sf()` from `sf` package will take the longitude and latitude and add a geometry column that we can use for  mapping. 

- We pass in longitude and latitude columns to coors argument. Must wrap longitude and latitude in quotes. 
- crs is 	coordinate reference system. 
- `remove=FALSE` will keep the cooridate columns in the dataframe


```r
temp <- inat %>% 
  st_as_sf(coords = c("longitude", "latitude"),   crs = 4326, remove=FALSE)
```

```error
Error in eval(expr, envir, enclos): object 'inat' not found
```

use select to pick which columns to use.


```r
inat_map <- inat %>% 
  st_as_sf(coords = c("longitude", "latitude"),   crs = 4326, remove=FALSE) %>% 
  select(id, user_login, common_name, scientific_name, observed_on,  url, longitude, latitude, geometry) 
```

```error
Error in eval(expr, envir, enclos): object 'inat' not found
```

```r
head(inat_map)
```

```error
Error in eval(expr, envir, enclos): object 'inat_map' not found
```

## static map

Use ggplot to plot the observations.


```r
ggplot() +
  geom_sf(data = inat_map)  
```

```error
Error in eval(expr, envir, enclos): object 'inat_map' not found
```
There are some observations that are outside of Los Angeles. Use filter to select observations in LA. 


```r
inat_map <- inat_map %>% 
  filter(latitude < 40)  
```

```error
Error in eval(expr, envir, enclos): object 'inat_map' not found
```

```r
ggplot() +
  geom_sf(data = inat_map)  
```

```error
Error in eval(expr, envir, enclos): object 'inat_map' not found
```
## interactive map

use mapview package to create interactive maps. 


```r
# mapview(inat_map)
```

Use `dim()` to show the number of rows and columns. There are over 90K rows, so the map is very slow. I suggest not using mapview if there are lots of rows.


```r
dim(inat_map)
```

```error
Error in eval(expr, envir, enclos): object 'inat_map' not found
```

To speed up the map, let's filter the list of observations. Get all observations for Western Fence Lizard.


```r
inat_lizard <- inat_map %>% 
  filter(common_name == 'Western Fence Lizard')
```

```error
Error in eval(expr, envir, enclos): object 'inat_map' not found
```

```r
dim(inat_lizard)
```

```error
Error in eval(expr, envir, enclos): object 'inat_lizard' not found
```

Create map. You can zoom in and out. Click on observation to see the info.


```r
mapview(inat_lizard)
```

```error
Error in eval(expr, envir, enclos): object 'inat_lizard' not found
```

## working with other geographic files

Let's add LA county boundaries to the map. use `read_sf()` from `sf` package to read the shape file.


```r
path = here('data', 'raw', 'County_Boundary', 'County_Boundary.shp')
la_county <- read_sf(path)
```

```error
Error: Cannot open "/home/runner/work/CNC-coding-workshop/CNC-coding-workshop/site/built/data/raw/County_Boundary/County_Boundary.shp"; The file doesn't seem to exist.
```

```r
la_county
```

```error
Error in eval(expr, envir, enclos): object 'la_county' not found
```
add LA County to maps.


```r
ggplot() +
  geom_sf(data = la_county)  +
  geom_sf(data = inat_lizard) 
```

```error
Error in eval(expr, envir, enclos): object 'la_county' not found
```


```r
mapview(la_county) +
  mapview(inat_lizard) 
```

```error
Error in eval(expr, envir, enclos): object 'la_county' not found
```


## Exploring iNaturlist data 

Lets look for all iNaturalist observations made in Exposition Park. 


```r
path = here('data', 'raw', 'boundaries_expo_park_area.geojson')
expo_park <- st_read(path)   %>% st_transform(4326)
```

```error
Error: Cannot open "/home/runner/work/CNC-coding-workshop/CNC-coding-workshop/site/built/data/raw/boundaries_expo_park_area.geojson"; The file doesn't seem to exist.
```

plot map of Expo Park.



```r
ggplot() +
  geom_sf(data = expo_park) 
```

```error
Error in eval(expr, envir, enclos): object 'expo_park' not found
```


```r
mapview(expo_park) 
```

```error
Error in eval(expr, envir, enclos): object 'expo_park' not found
```

We want to get observation inside Expo Park.

You should check if the crs for the inaturalist data and the Expo Park are the same


```r
st_crs(expo_park) == st_crs(inat_map)
```

```error
Error in eval(expr, envir, enclos): object 'expo_park' not found
```

Use `st_intersection()` to get  all observations that inside of Exposition Park.


```r
inat_expo <- inat_map %>% st_intersection(expo_park)
```

```error
Error in eval(expr, envir, enclos): object 'inat_map' not found
```
Use dim to get row and column count.  93K in LA county.  1191 observation in Expo Park.


```r
dim(inat_map)
```

```error
Error in eval(expr, envir, enclos): object 'inat_map' not found
```

```r
dim(inat_expo)
```

```error
Error in eval(expr, envir, enclos): object 'inat_expo' not found
```

Create map of all observations in Expo Park.


```r
ggplot() +
  geom_sf(data = expo_park)  +
  geom_sf(data = inat_expo) 
```

```error
Error in eval(expr, envir, enclos): object 'expo_park' not found
```


```r
mapview(expo_park) +
  mapview(inat_expo) 
```

```error
Error in eval(expr, envir, enclos): object 'expo_park' not found
```

Use alpha.regions to set opacity.
use col.regions to set color.


```r
mapview(expo_park,   alpha.regions=0.3, col.regions="#333333") +
  mapview(inat_expo) 
```

```error
Error in eval(expr, envir, enclos): object 'expo_park' not found
```

