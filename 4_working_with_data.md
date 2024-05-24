---
title: "Working with data"
teaching: 120
exercises: 4
output: html_document

---



  
:::::::::::::::::::::::::::::::::::::: questions 

- How do you manipulate tabular data in R?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Import CSV data into R.
 - Use pipes to link steps together into pipelines.
 - Export data to a CSV file.

::::::::::::::::::::::::::::::::::::::::::::::::
  


## R Packages

R packages are extensions to the R language. R packages contain code, data, and documentation that people can download and install to add more functionality to R. 

To download and install packages onto your computer, type `install.packages("package_name")` in the R console. Must use quotes. This function will connect to the internet and download packages from servers that have R packages. The Comprehensive R Archive Network (cran.r-project.org) is a network of web servers around the world that store R packages.  

To use the package, use `library(package_name)` to load it. Do not use quotes. You want to install the package to your computer once, and then load it with `library()` in each script where you need to use it. Generally its a good idea to list all the libraries at the beginning of the script.


### `tidyverse`

`tidyverse` is collection of R packages that are used for analyzing data.  These packages like data in "tidy" format, which means each column represents a single field, and each row represents a single record

## Importing data

#### File paths

When we reference other files from an R script, we need to give R precise instructions on where those files are. We do that using something called a **file path**.   

There are two kinds of paths: **absolute** and **relative**. Absolute paths are specific to a particular computer, whereas relative paths are relative to a certain folder.  For instance an absolute path is "/Users/wyk/Documents/code_stuff/CNC_coding_intro_lesson", and relative path is "CNC_coding_intro_lesson". 


#### Read a file

Use library to load the needed packages.


```r
library(readr)
library(lubridate)
library(dplyr)
```


We will use the `read_csv` function from `readr` package to read a csv of CNC iNaturalist observations, and the argument we give will be the path to the CSV file. We will store the observations in an object named `inat_raw`.



```r
inat_raw <- read_csv('data/raw/observations-397280.csv')
```

```output
Rows: 171155 Columns: 39
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr  (23): observed_on_string, time_observed_at, time_zone, user_login, user...
dbl  (10): id, user_id, num_identification_agreements, num_identification_di...
lgl   (5): captive_cultivated, private_place_guess, private_latitude, privat...
date  (1): observed_on

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


`inat_raw` is stored in memory. It appears in **Environment** tab. Double click on `inat_raw` in **Environment** to see all the data.


`read_csv` provides some info about the CSV. 

- the number of rows and columns
- the **delimiter** of the file, which is how values are separated, a comma `","`
- the data types for the columns



Use `glimpse()` to see a information about a dataframe. Number of rows and columns. For each column, we see the name, data type (**dbl** for number, **chr** for character, **lgl** for logical. **date** is a data type from data.frame), and the first few values.  

 

```r
glimpse(inat_raw)
```

```output
Rows: 171,155
Columns: 39
$ id                               <dbl> 2931940, 2934641, 2934854, 2934961, 2…
$ observed_on_string               <chr> "2016-04-14 12:25:00 AM PDT", "Thu Ap…
$ observed_on                      <date> 2016-04-14, 2016-04-14, 2016-04-14, …
$ time_observed_at                 <chr> "2016-04-14 19:25:00 UTC", "2016-04-1…
$ time_zone                        <chr> "Pacific Time (US & Canada)", "Pacifi…
$ user_id                          <dbl> 151043, 10814, 8510, 80445, 80445, 80…
$ user_login                       <chr> "msmorales", "smartrf", "stonebird", …
$ user_name                        <chr> "Michael Morales", "Richard Smart (he…
$ created_at                       <chr> "2016-04-14 07:28:36 UTC", "2016-04-1…
$ updated_at                       <chr> "2021-12-26 06:58:04 UTC", "2018-05-2…
$ quality_grade                    <chr> "research", "needs_id", "needs_id", "…
$ license                          <chr> "CC-BY", "CC-BY-NC", NA, NA, NA, NA, …
$ url                              <chr> "http://www.inaturalist.org/observati…
$ image_url                        <chr> "https://inaturalist-open-data.s3.ama…
$ sound_url                        <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
$ tag_list                         <chr> NA, NA, "\"Allen's Hummingbird\" \"Se…
$ description                      <chr> "Spotted on a the wall of a planter, …
$ num_identification_agreements    <dbl> 5, 2, 0, 1, 2, 2, 1, 0, 1, 2, 1, 1, 1…
$ num_identification_disagreements <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
$ captive_cultivated               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FA…
$ oauth_application_id             <dbl> 2, 3, NA, NA, NA, NA, 3, 3, NA, NA, N…
$ place_guess                      <chr> "Olive Lane Walk Pomona, CA 91768", "…
$ latitude                         <dbl> 34.05829, 34.01742, NA, 34.13020, 34.…
$ longitude                        <dbl> -117.8219, -118.2892, NA, -118.8226, …
$ positional_accuracy              <dbl> 4, 5, 220, NA, NA, NA, NA, 17, 55, 55…
$ private_place_guess              <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
$ private_latitude                 <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
$ private_longitude                <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
$ public_positional_accuracy       <dbl> 4, 5, 28888, NA, NA, NA, NA, 17, 55, …
$ geoprivacy                       <chr> NA, NA, "private", NA, NA, NA, NA, NA…
$ taxon_geoprivacy                 <chr> NA, NA, "open", NA, "open", "open", N…
$ coordinates_obscured             <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FAL…
$ positioning_method               <chr> "gps", NA, NA, NA, NA, NA, NA, NA, NA…
$ positioning_device               <chr> "gps", NA, NA, NA, NA, NA, NA, NA, NA…
$ species_guess                    <chr> "Garden Snail", "Oestroidea", "Allen'…
$ scientific_name                  <chr> "Cornu aspersum", "Oestroidea", "Sela…
$ common_name                      <chr> "Garden Snail", "Bot Flies, Blow Flie…
$ iconic_taxon_name                <chr> "Mollusca", "Insecta", "Aves", "Insec…
$ taxon_id                         <dbl> 480298, 356157, 6359, 54247, 36100, 3…
```
 

`nrow()` returns the number of rows. 
`ncol()` returns the number of columns 
`dim()` returns the number of rows and columns. 


```r
nrow(inat_raw)
```

```output
[1] 171155
```

```r
ncol(inat_raw)
```

```output
[1] 39
```

```r
dim(inat_raw)
```

```output
[1] 171155     39
```


`names()` shows the column names


```r
names(inat_raw)
```

```output
 [1] "id"                               "observed_on_string"              
 [3] "observed_on"                      "time_observed_at"                
 [5] "time_zone"                        "user_id"                         
 [7] "user_login"                       "user_name"                       
 [9] "created_at"                       "updated_at"                      
[11] "quality_grade"                    "license"                         
[13] "url"                              "image_url"                       
[15] "sound_url"                        "tag_list"                        
[17] "description"                      "num_identification_agreements"   
[19] "num_identification_disagreements" "captive_cultivated"              
[21] "oauth_application_id"             "place_guess"                     
[23] "latitude"                         "longitude"                       
[25] "positional_accuracy"              "private_place_guess"             
[27] "private_latitude"                 "private_longitude"               
[29] "public_positional_accuracy"       "geoprivacy"                      
[31] "taxon_geoprivacy"                 "coordinates_obscured"            
[33] "positioning_method"               "positioning_device"              
[35] "species_guess"                    "scientific_name"                 
[37] "common_name"                      "iconic_taxon_name"               
[39] "taxon_id"                        
```


To access one column, use `$` and name of the column 


```r
inat_raw$quality_grade
```

```output
  [1] "research" "needs_id" "needs_id" "research" "research" "research"
  [7] "needs_id" "needs_id" "casual"   "research" "research" "research"
 [13] "needs_id" "research" "research" "research" "research" "research"
 [19] "research" "research" "needs_id" "research" "research" "casual"  
 [25] "casual"   "casual"   "casual"   "research" "casual"   "casual"  
 [31] "casual"   "research" "research" "casual"   "research" "needs_id"
 [37] "needs_id" "research" "research" "research" "research" "research"
 [43] "casual"   "needs_id" "research" "research" "research" "needs_id"
 [49] "research" "casual"   "casual"   "casual"   "research" "research"
 [55] "needs_id" "research" "research" "research" "needs_id" "needs_id"
 [61] "research" "needs_id" "research" "research" "casual"   "needs_id"
 [67] "research" "research" "needs_id" "research" "research" "research"
 [73] "needs_id" "needs_id" "research" "research" "casual"   "research"
 [79] "research" "research" "needs_id" "research" "casual"   "needs_id"
 [85] "research" "research" "research" "research" "research" "research"
 [91] "needs_id" "research" "research" "casual"   "research" "needs_id"
 [97] "needs_id" "research" "research" "needs_id"
 [ reached getOption("max.print") -- omitted 171055 entries ]
```


To view all the unique values in a column, use `unique()`


```r
unique(inat_raw$quality_grade)
```

```output
[1] "research" "needs_id" "casual"  
```





## Manipulating data

One of the most important skills for working with data in R is the ability to manipulate, modify, and reshape data. The `dplyr` package provide a series of powerful functions for many common data manipulation tasks.

select()
filter()
mutate()
arrange()
count()


#### `select()`

`select()` picks certain columns of a data.frame. To use the `select()` function, the first argument is the name of the data.frame, and the rest of the arguments are *unquoted* names of the columns you want.

iNaturalist has 39 columns. We want four columns. The columns are arranged in the order we specified inside `select()`.



```r
select(inat_raw, user_login, common_name, scientific_name, observed_on)
```

```output
# A tibble: 171,155 × 4
   user_login    common_name                         scientific_name observed_on
   <chr>         <chr>                               <chr>           <date>     
 1 msmorales     Garden Snail                        Cornu aspersum  2016-04-14 
 2 smartrf       Bot Flies, Blow Flies, and Allies   Oestroidea      2016-04-14 
 3 stonebird     Allen's Hummingbird                 Selasphorus sa… 2016-04-14 
 4 cdegroof      California Orange-winged Grasshopp… Arphia ramona   2016-04-14 
 5 cdegroof      Western Side-blotched Lizard        Uta stansburia… 2016-04-14 
 6 cdegroof      Western Fence Lizard                Sceloporus occ… 2016-04-14 
 7 ttempel       <NA>                                Coelocnemis     2016-04-14 
 8 bradrumble    House Sparrow                       Passer domesti… 2016-04-15 
 9 deedeeflower5 Amur Carp                           Cyprinus rubro… 2016-04-14 
10 deedeeflower5 Red-eared Slider                    Trachemys scri… 2016-04-14 
# ℹ 171,145 more rows
```


#### `filter()`    


The `filter()` function is used to select rows that meet certain criteria. To get all the rows where the value of `common_name` is equal to `Western Fence Lizard`, we would run the following:




```r
filter(inat_raw, common_name == 'Western Fence Lizard')
```

```output
# A tibble: 2,970 × 39
        id observed_on_string     observed_on time_observed_at time_zone user_id
     <dbl> <chr>                  <date>      <chr>            <chr>       <dbl>
 1 2934994 2016-04-14 12:19:09    2016-04-14  2016-04-14 19:1… Pacific …   80445
 2 2935263 2016-04-14             2016-04-14  <NA>             Pacific …  216108
 3 2935420 2016-04-14             2016-04-14  <NA>             Pacific …  216108
 4 2935748 2016-04-14 14:01:29    2016-04-14  2016-04-14 21:0… Pacific …   80445
 5 2935965 Thu Apr 14 2016 12:44… 2016-04-14  2016-04-14 19:4… Pacific …  171443
 6 2938607 Thu Apr 14 2016 16:33… 2016-04-14  2016-04-14 23:3… Pacific …  146517
 7 2940103 2016-04-15 9:31:39 AM… 2016-04-15  2016-04-15 16:3… Pacific …   80984
 8 2940838 Fri Apr 15 2016 10:11… 2016-04-15  2016-04-15 17:1… Pacific …  201119
 9 2940848 Fri Apr 15 2016 10:17… 2016-04-15  2016-04-15 17:1… Pacific …  201119
10 2940855 Fri Apr 15 2016 10:42… 2016-04-15  2016-04-15 17:4… Pacific …  201119
# ℹ 2,960 more rows
# ℹ 33 more variables: user_login <chr>, user_name <chr>, created_at <chr>,
#   updated_at <chr>, quality_grade <chr>, license <chr>, url <chr>,
#   image_url <chr>, sound_url <chr>, tag_list <chr>, description <chr>,
#   num_identification_agreements <dbl>,
#   num_identification_disagreements <dbl>, captive_cultivated <lgl>,
#   oauth_application_id <dbl>, place_guess <chr>, latitude <dbl>, …
```

The `==` sign means "is equal to". There are several other operators we can use: >, >=, <, <=, and != (not equal to). 

## The pipe: `%>%`

What happens if we want to both `select()` and `filter()` our data? 

We use the pipe operator (`%>%`) to call multiple functions. You can insert it by using the keyboard shortcut <kbd>Shift+Cmd+M</kbd> (Mac) or <kbd>Shift+Ctrl+M</kbd> (Windows). 

Get  user_login, common_name, scientific_name, observed_on for all observations where common_name is  'Western Fence Lizard'. Use filter to select rows, then use select to select columns.


```r
inat_raw %>% 
  filter(common_name == 'Western Fence Lizard') %>% 
  select(user_login, common_name, scientific_name, observed_on) 
```

```output
# A tibble: 2,970 × 4
   user_login    common_name          scientific_name         observed_on
   <chr>         <chr>                <chr>                   <date>     
 1 cdegroof      Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 2 deedeeflower5 Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 3 deedeeflower5 Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 4 cdegroof      Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 5 lchroman      Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 6 maiz          Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 7 kimssight     Western Fence Lizard Sceloporus occidentalis 2016-04-15 
 8 sarahwenner   Western Fence Lizard Sceloporus occidentalis 2016-04-15 
 9 sarahwenner   Western Fence Lizard Sceloporus occidentalis 2016-04-15 
10 sarahwenner   Western Fence Lizard Sceloporus occidentalis 2016-04-15 
# ℹ 2,960 more rows
```

Pipe operator take the thing on the lefthand side and insert it as the first argument of the function on the righthand side. By putting each of our functions onto a new line, we can build a nice, readable pipeline. It can be useful to think of this as a little assembly line for our data. It starts at the top and gets piped into a `filter()` function, and it comes out modified somewhat. It then gets sent into the `select()` function, where it is further modified, and then the final product gets printed out to our console. It can also be helpful to think of `%>%` as meaning "and then".  

If you want to see all the records, assign the data.frame to an object.


```r
temp <- inat_raw %>% 
  filter(common_name == 'Western Fence Lizard') %>% 
  select(user_login, common_name, scientific_name, observed_on) 
```

We can also use multiple conditions in one `filter()` statement. 

When researchers use iNaturalist data, the normally use research grade observations. Here we will get all observations that research grade and common_name is Western Fence Lizard. use `&` for and.
 

```r
inat_raw %>% 
  filter( common_name == 'Western Fence Lizard' 
         & quality_grade == 'research')  %>% 
  select(user_login, common_name, scientific_name, observed_on)
```

```output
# A tibble: 2,942 × 4
   user_login    common_name          scientific_name         observed_on
   <chr>         <chr>                <chr>                   <date>     
 1 cdegroof      Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 2 deedeeflower5 Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 3 deedeeflower5 Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 4 cdegroof      Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 5 lchroman      Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 6 maiz          Western Fence Lizard Sceloporus occidentalis 2016-04-14 
 7 kimssight     Western Fence Lizard Sceloporus occidentalis 2016-04-15 
 8 sarahwenner   Western Fence Lizard Sceloporus occidentalis 2016-04-15 
 9 sarahwenner   Western Fence Lizard Sceloporus occidentalis 2016-04-15 
10 sarahwenner   Western Fence Lizard Sceloporus occidentalis 2016-04-15 
# ℹ 2,932 more rows
```


Here we will get observations  where `user_login` is 'natureinla' and `common_name` is 'Western Fence Lizard'. 



```r
inat_raw %>% 
  filter(user_login == 'natureinla' & common_name == 'Western Fence Lizard') %>% 
  select(user_login, common_name, scientific_name, observed_on) 
```

```output
# A tibble: 79 × 4
   user_login common_name          scientific_name         observed_on
   <chr>      <chr>                <chr>                   <date>     
 1 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-16 
 2 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-16 
 3 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-17 
 4 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-16 
 5 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-17 
 6 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-17 
 7 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-19 
 8 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-16 
 9 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-18 
10 natureinla Western Fence Lizard Sceloporus occidentalis 2016-04-16 
# ℹ 69 more rows
```



Here we will get observations  where  `common_name` is 'Western Fence Lizard' or 'Western Honey Bee'. use `|` for or. 


```r
inat_raw %>% 
  filter(common_name == 'Western Honey Bee' | common_name == 'Western Fence Lizard')  %>% 
  select(user_login, observed_on, common_name)
```

```output
# A tibble: 4,788 × 3
   user_login    observed_on common_name         
   <chr>         <date>      <chr>               
 1 cdegroof      2016-04-14  Western Fence Lizard
 2 deedeeflower5 2016-04-14  Western Fence Lizard
 3 deedeeflower5 2016-04-14  Western Fence Lizard
 4 cdegroof      2016-04-14  Western Fence Lizard
 5 lchroman      2016-04-14  Western Fence Lizard
 6 smartrf       2016-04-14  Western Honey Bee   
 7 maiz          2016-04-14  Western Fence Lizard
 8 smartrf       2016-04-15  Western Honey Bee   
 9 kimssight     2016-04-15  Western Fence Lizard
10 catherineh    2016-04-15  Western Honey Bee   
# ℹ 4,778 more rows
```


Sometimes we want to combine and or. We want observations  from 'cdegroof' or 'deedeeflower5' for 'Western Fence Lizard'. You can use both `&` and `|` together in a single filter.


```r
temp <- inat_raw %>% 
  filter(user_login == 'cdegroof' 
         | user_login == 'deedeeflower5'
         & common_name == 'Western Fence Lizard')  %>% 
  select(user_login, common_name, scientific_name, observed_on)
```

You can also use multiple filter statememts.


```r
temp <- inat_raw %>% 
  filter(user_login == 'cdegroof' 
         | user_login == 'deedeeflower5') %>%
  filter(common_name == 'Western Fence Lizard')  %>% 
  select(user_login, observed_on, common_name)
```


## Cleaning up raw data, exporting dataframe

A common step during data analysis is to clean up the raw data. We fix any obvious errors, edit column names, exclude rows we do not want, and save the cleaned up data set. We do the analysis on the cleaned data set.

We want observation that match these criteria
- have a species information.
- have latitude or longitude.
- have 'research' for quality_grade


Use `colSums(is.na())` to count the number of rows that have NA values for each column.


```r
colSums(is.na(inat_raw))
```

```output
                              id               observed_on_string 
                               0                                0 
                     observed_on                 time_observed_at 
                               0                             5819 
                       time_zone                          user_id 
                               0                                0 
                      user_login                        user_name 
                               0                            63304 
                      created_at                       updated_at 
                               0                                0 
                   quality_grade                          license 
                               0                            42937 
                             url                        image_url 
                               0                             2214 
                       sound_url                         tag_list 
                          170645                           164464 
                     description    num_identification_agreements 
                          149235                                0 
num_identification_disagreements               captive_cultivated 
                               0                                0 
            oauth_application_id                      place_guess 
                           66613                              440 
                        latitude                        longitude 
                             438                              438 
             positional_accuracy              private_place_guess 
                           38319                           171155 
                private_latitude                private_longitude 
                          171155                           171155 
      public_positional_accuracy                       geoprivacy 
                           34911                           158521 
                taxon_geoprivacy             coordinates_obscured 
                          129409                                0 
              positioning_method               positioning_device 
                          156084                           154467 
                   species_guess                  scientific_name 
                           25721                             1685 
                     common_name                iconic_taxon_name 
                           11164                             1846 
                        taxon_id 
                            1685 
```
All rows have id, observed_on, and user_id.

1685 rows don't have scientific_name. 438 rows don't have latitude or longitude.

`table` is a function from base R that can count the number of unique values in a column. Get a count for `quality_grade`.


```r
table(inat_raw$quality_grade)
```

```output

  casual needs_id research 
   23194    53875    94086 
```

94086 rows are `research` grade.

use filter to select the observations we want. 

`!is.na` will select rows that have are not NA, meaning rows that have a value.
`quality_grade == 'research'` will select rows that are 'research' grade.

save the cleaned up data in a new object `inat`.


```r
inat <- inat_raw %>% 
  filter(!is.na(latitude) &
           !is.na(longitude) &
           !is.na(scientific_name)) %>% 
  filter(quality_grade == 'research')
```

The original dataframe 'inat_raw' had 171K rows, the cleaned dataframe 'inat' has 93K rows. 

We can double check our work.

latitude, longitude, scientific_name have zero NA.


```r
colSums(is.na(inat))
```

```output
                              id               observed_on_string 
                               0                                0 
                     observed_on                 time_observed_at 
                               0                             3167 
                       time_zone                          user_id 
                               0                                0 
                      user_login                        user_name 
                               0                            30889 
                      created_at                       updated_at 
                               0                                0 
                   quality_grade                          license 
                               0                            21944 
                             url                        image_url 
                               0                              334 
                       sound_url                         tag_list 
                           93561                            89612 
                     description    num_identification_agreements 
                           82364                                0 
num_identification_disagreements               captive_cultivated 
                               0                                0 
            oauth_application_id                      place_guess 
                           41638                                1 
                        latitude                        longitude 
                               0                                0 
             positional_accuracy              private_place_guess 
                           22494                            93950 
                private_latitude                private_longitude 
                           93950                            93950 
      public_positional_accuracy                       geoprivacy 
                           20663                            87462 
                taxon_geoprivacy             coordinates_obscured 
                           59216                                0 
              positioning_method               positioning_device 
                           84974                            84450 
                   species_guess                  scientific_name 
                             138                                0 
                     common_name                iconic_taxon_name 
                            1626                                4 
                        taxon_id 
                               0 
```

quality_grade only has research.

```r
table(inat$quality_grade)
```

```output

research 
   93950 
```

We want to save the cleaned up data set so we can use it later.  We can save data.frame to a CSV using the `write_csv()` function from the `readr` package. The first argument is the name of the data.frame, and the second is the path to the new file we want to create, including the file extension `.csv`.



```r
write_csv(inat, file= 'data/cleaned/observations.csv')
```


If we go look into our `cleaned_data` folder, we will see this new CSV file.


## Errors in code

We are writing instructions for the computer. If there is typos, mispelling, pass in wrong arguments into functions, etc, code will not work and we will see errors. R will display the errors in red.

typo for `%>`


```r
inat %>%
  select(user_login, observed_on, common_name) %>% 
  filter(user_login == 'natureinla')
```

```output
# A tibble: 1,528 × 3
   user_login observed_on common_name           
   <chr>      <date>      <chr>                 
 1 natureinla 2016-04-14  Red-eared Slider      
 2 natureinla 2016-04-14  Monarch               
 3 natureinla 2016-04-14  San Diego Gopher Snake
 4 natureinla 2016-04-14  California Towhee     
 5 natureinla 2016-04-14  Cooper's Hawk         
 6 natureinla 2016-04-14  Monarch               
 7 natureinla 2016-04-14  Allen's Hummingbird   
 8 natureinla 2016-04-15  Northern Mockingbird  
 9 natureinla 2016-04-15  House Sparrow         
10 natureinla 2016-04-15  Indian Peafowl        
# ℹ 1,518 more rows
```

Misspelled `user_logi`


```r
inat %>%
  select(user_login, observed_on, common_name) %>% 
  filter(user_login == 'natureinla')
```

```output
# A tibble: 1,528 × 3
   user_login observed_on common_name           
   <chr>      <date>      <chr>                 
 1 natureinla 2016-04-14  Red-eared Slider      
 2 natureinla 2016-04-14  Monarch               
 3 natureinla 2016-04-14  San Diego Gopher Snake
 4 natureinla 2016-04-14  California Towhee     
 5 natureinla 2016-04-14  Cooper's Hawk         
 6 natureinla 2016-04-14  Monarch               
 7 natureinla 2016-04-14  Allen's Hummingbird   
 8 natureinla 2016-04-15  Northern Mockingbird  
 9 natureinla 2016-04-15  House Sparrow         
10 natureinla 2016-04-15  Indian Peafowl        
# ℹ 1,518 more rows
```

typo for `=`


```r
inat %>%
  select(user_login, observed_on, common_name) %>% 
  filter(user_login == 'natureinla')
```

```output
# A tibble: 1,528 × 3
   user_login observed_on common_name           
   <chr>      <date>      <chr>                 
 1 natureinla 2016-04-14  Red-eared Slider      
 2 natureinla 2016-04-14  Monarch               
 3 natureinla 2016-04-14  San Diego Gopher Snake
 4 natureinla 2016-04-14  California Towhee     
 5 natureinla 2016-04-14  Cooper's Hawk         
 6 natureinla 2016-04-14  Monarch               
 7 natureinla 2016-04-14  Allen's Hummingbird   
 8 natureinla 2016-04-15  Northern Mockingbird  
 9 natureinla 2016-04-15  House Sparrow         
10 natureinla 2016-04-15  Indian Peafowl        
# ℹ 1,518 more rows
```

extra `)`


```r
inat %>%
  select(user_login, observed_on, common_name) %>% 
  filter (user_login == 'natureinla')
```

```output
# A tibble: 1,528 × 3
   user_login observed_on common_name           
   <chr>      <date>      <chr>                 
 1 natureinla 2016-04-14  Red-eared Slider      
 2 natureinla 2016-04-14  Monarch               
 3 natureinla 2016-04-14  San Diego Gopher Snake
 4 natureinla 2016-04-14  California Towhee     
 5 natureinla 2016-04-14  Cooper's Hawk         
 6 natureinla 2016-04-14  Monarch               
 7 natureinla 2016-04-14  Allen's Hummingbird   
 8 natureinla 2016-04-15  Northern Mockingbird  
 9 natureinla 2016-04-15  House Sparrow         
10 natureinla 2016-04-15  Indian Peafowl        
# ℹ 1,518 more rows
```

::::::::::::::::::::::::::::::::::::: challenge

## Exercise 1

Get  your observations for  one species. 
- The data.frame should have `user_login`, `observed_on`,  `common-name`
- Use `select()`, `filter()`, `&`  
::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution


```r
inat %>% 
  filter(user_login == 'natureinla' & common_name == 'Red-eared Slider') %>% 
  select(user_login, observed_on, common_name) 
```

```output
# A tibble: 13 × 3
   user_login observed_on common_name     
   <chr>      <date>      <chr>           
 1 natureinla 2016-04-14  Red-eared Slider
 2 natureinla 2016-04-14  Red-eared Slider
 3 natureinla 2017-04-15  Red-eared Slider
 4 natureinla 2017-04-15  Red-eared Slider
 5 natureinla 2017-04-16  Red-eared Slider
 6 natureinla 2017-04-14  Red-eared Slider
 7 natureinla 2017-04-17  Red-eared Slider
 8 natureinla 2017-04-18  Red-eared Slider
 9 natureinla 2017-04-18  Red-eared Slider
10 natureinla 2017-04-14  Red-eared Slider
11 natureinla 2018-04-30  Red-eared Slider
12 natureinla 2018-04-30  Red-eared Slider
13 natureinla 2019-04-27  Red-eared Slider
```

::::::::::::::::::::::::::::::::::::::::::::::::


## Making new columns with `mutate()`

Another common task is creating a new column based on values in existing columns. For example, we could add a new column for year.

Use `mutate()` to a column. We pass in the name of the new column, and the value of the column. 

Use `year()` from `lubridate` on a date column to get the year. 

This code will get the year from 'observed_on', and create a 'year' column.



```r
temp <- inat %>% 
  mutate(year = year(observed_on))
```

Get observations for 2020.


```r
inat %>% 
  mutate(year = year(observed_on)) %>%
  filter(year == 2020)
```

```output
# A tibble: 10,659 × 40
         id observed_on_string    observed_on time_observed_at time_zone user_id
      <dbl> <chr>                 <date>      <chr>            <chr>       <dbl>
 1 43036534 Fri Apr 24 2020 00:0… 2020-04-24  2020-04-24 07:0… Pacific …  146517
 2 43036989 Fri Apr 24 2020 00:0… 2020-04-24  2020-04-24 07:0… Pacific …   74669
 3 43037631 Fri Apr 24 2020 00:1… 2020-04-24  2020-04-24 07:1… Pacific …  403949
 4 43037703 Fri Apr 24 2020 00:1… 2020-04-24  2020-04-24 07:1… Pacific …  403949
 5 43037736 Fri Apr 24 2020 00:0… 2020-04-24  2020-04-24 07:0… Pacific …  403949
 6 43037745 Fri Apr 24 2020 00:1… 2020-04-24  2020-04-24 07:1… Pacific … 2556338
 7 43037824 2020-04-24 12:05:06 … 2020-04-24  2020-04-24 07:0… Pacific … 1628946
 8 43037956 Fri Apr 24 2020 00:1… 2020-04-24  2020-04-24 07:1… Pacific …   74669
 9 43037961 Fri Apr 24 2020 00:2… 2020-04-24  2020-04-24 07:2… Pacific … 2556338
10 43038195 Fri Apr 24 2020 00:2… 2020-04-24  2020-04-24 07:2… Pacific … 2556338
# ℹ 10,649 more rows
# ℹ 34 more variables: user_login <chr>, user_name <chr>, created_at <chr>,
#   updated_at <chr>, quality_grade <chr>, license <chr>, url <chr>,
#   image_url <chr>, sound_url <chr>, tag_list <chr>, description <chr>,
#   num_identification_agreements <dbl>,
#   num_identification_disagreements <dbl>, captive_cultivated <lgl>,
#   oauth_application_id <dbl>, place_guess <chr>, latitude <dbl>, …
```

::::::::::::::::::::::::::::::::::::: challenge

## Exercise 2

1. Create a data.frame with all of your observations from the last year. 

- Use `select()` , `filter()`
- Use `mutate()` and `year()` to add year column
- The data.frame should have `user_login`, `observed_on`, and `common-name`. 
::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::: solution


```r
inat %>% 
   mutate(year = year(observed_on)) %>%
  filter(user_login == 'natureinla' & year == 2023) %>%
  select(user_login, observed_on, common_name) 
```

```output
# A tibble: 3 × 3
  user_login observed_on common_name             
  <chr>      <date>      <chr>                   
1 natureinla 2023-04-29  Thick-leaved Yerba Santa
2 natureinla 2023-04-29  Big Berry Manzanita     
3 natureinla 2023-04-29  chamise                 
```
 
::::::::::::::::::::::::::::::::::::::::::::::::

## Count the number of rows with `count()`


Use `count()` from dplyr to count the number of values for one or more columns.

Let's try counting of all our observations by year. Use `mutate` to add a year column. Use `count` to count the number of observations for each year. By default, count will add a new column caled `n`. 


```r
inat %>% 
  mutate(year = year(observed_on)) %>%
  count(year)  
```

```output
# A tibble: 8 × 2
   year     n
  <dbl> <int>
1  2016  5791
2  2017  9354
3  2018 10855
4  2019 17950
5  2020 10659
6  2021 13051
7  2022 11924
8  2023 14366
```
We can specify the name of the count column by passing in  `name` to `count()` 


```r
inat %>% 
  mutate(year = year(observed_on)) %>%
  count(year, name='obs_count')  
```

```output
# A tibble: 8 × 2
   year obs_count
  <dbl>     <int>
1  2016      5791
2  2017      9354
3  2018     10855
4  2019     17950
5  2020     10659
6  2021     13051
7  2022     11924
8  2023     14366
```

Let's count the number of observations for each species. We will pass in both 'common_name' and 'scientific_name' because some species don't have a common_name.


```r
inat %>% 
  count(common_name, scientific_name, name='obs_count')   
```

```output
# A tibble: 3,675 × 3
   common_name                 scientific_name         obs_count
   <chr>                       <chr>                       <int>
 1 Abert's Thread-waisted Wasp Ammophila aberti                1
 2 Accipiters                  Accipiter                       2
 3 Acmon Blue                  Icaricia acmon                 35
 4 Acorn Woodpecker            Melanerpes formicivorus       256
 5 Acton's Brittlebush         Encelia actoni                 11
 6 Acute Bladder Snail         Physella acuta                  8
 7 Adams Mussel                Brachidontes adamsianus         4
 8 African Asparagus           Asparagus aethiopicus          22
 9 African Clawed Frog         Xenopus laevis                  1
10 African Cluster Bug         Agonoscelis puberula           13
# ℹ 3,665 more rows
```

 It's often useful to take a look at the results in some order, like the lowest count to highest. We can use the `arrange()` function for that. By default, arrange will return values from lowest to highest.
 
 

```r
inat %>% 
  count(common_name, scientific_name, name='obs_count')   %>%
  arrange(obs_count)
```

```output
# A tibble: 3,675 × 3
   common_name                        scientific_name               obs_count
   <chr>                              <chr>                             <int>
 1 Abert's Thread-waisted Wasp        Ammophila aberti                      1
 2 African Clawed Frog                Xenopus laevis                        1
 3 African boxthorn                   Lycium ferocissimum                   1
 4 Almond                             Prunus amygdalus                      1
 5 Alpine Brown Sunken Disk Lichen    Bellemerea alpina                     1
 6 American Black-crowned Night Heron Nycticorax nycticorax hoactli         1
 7 American Dewdrop Spider            Argyrodes elevatus                    1
 8 American Dipper                    Cinclus mexicanus                     1
 9 American Softshells                Apalone                               1
10 American Sunflower Moth            Homoeosoma electella                  1
# ℹ 3,665 more rows
```


If we want to reverse the order, we can wrap the column name in `desc()`:


```r
inat %>% 
  count(common_name, scientific_name, name='obs_count') %>%
  arrange(desc(obs_count)) 
```

```output
# A tibble: 3,675 × 3
   common_name            scientific_name         obs_count
   <chr>                  <chr>                       <int>
 1 Western Fence Lizard   Sceloporus occidentalis      2936
 2 Western Honey Bee      Apis mellifera               1803
 3 Fox Squirrel           Sciurus niger                1285
 4 House Finch            Haemorhous mexicanus         1067
 5 Mourning Dove          Zenaida macroura             1034
 6 Mallard                Anas platyrhynchos            810
 7 House Sparrow          Passer domesticus             800
 8 Convergent Lady Beetle Hippodamia convergens         788
 9 California Towhee      Melozone crissalis            747
10 Northern Mockingbird   Mimus polyglottos             719
# ℹ 3,665 more rows
```

use `slice()` to return only certain number of records
slice(start:end)

Top ten species with the most observations.


```r
inat %>% 
  count(common_name, scientific_name, name='obs_count') %>%
  arrange(desc(obs_count))  %>% 
  slice(1:10)
```

```output
# A tibble: 10 × 3
   common_name            scientific_name         obs_count
   <chr>                  <chr>                       <int>
 1 Western Fence Lizard   Sceloporus occidentalis      2936
 2 Western Honey Bee      Apis mellifera               1803
 3 Fox Squirrel           Sciurus niger                1285
 4 House Finch            Haemorhous mexicanus         1067
 5 Mourning Dove          Zenaida macroura             1034
 6 Mallard                Anas platyrhynchos            810
 7 House Sparrow          Passer domesticus             800
 8 Convergent Lady Beetle Hippodamia convergens         788
 9 California Towhee      Melozone crissalis            747
10 Northern Mockingbird   Mimus polyglottos             719
```


::::::::::::::::::::::::::::::::::::: challenge

## Exercise 3

1. Create a data.frame with that counts your observation by year

- Use `filter()` and `count()`
- Use `mutate()` and `year()` to add year column
::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::: solution


```r
inat %>% 
  mutate(year = year(observed_on)) %>%
  filter(user_login == 'natureinla') %>%
  count(year, name='obs_count')   
```

```output
# A tibble: 7 × 2
   year obs_count
  <dbl>     <int>
1  2016       490
2  2017       606
3  2018       223
4  2019       195
5  2020         9
6  2021         2
7  2023         3
```
::::::::::::::::::::::::::::::::::::::::::::::::


