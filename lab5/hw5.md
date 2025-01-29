---
title: "Homework 5"
author: "Jenna Hussein"
date: "2025-01-28"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and/or complete the exercises in RMarkdown. Please embed all of your code and push the final work to your repository. Your report should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run.  

## Load the tidyverse

``` r
library("tidyverse")
library("janitor")
```

## Data 
For this assignment, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the data folder.   

**1. Load `IvindoData_DryadVersion.csv` and store it as a new object called `gabon`.**

``` r
gabon <- read.csv("data/IvindoData_DryadVersion.csv")
```

**2. Use one or more of the summary functions you have learned to get an idea of the structure of the data.**  

``` r
glimpse(gabon)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <int> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16, …
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.06…
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "N…
## $ NumHouseholds           <int> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 56…
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "Pa…
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.7…
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.2…
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, 8…
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.2…
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.00…
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.71…
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.1…
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.6…
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.43…
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.2…
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.26…
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, 8…
## $ Rich_AllSpecies         <int> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 22…
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.81…
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.56…
## $ Rich_BirdSpecies        <int> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, 1…
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.80…
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.92…
## $ Rich_MammalSpecies      <int> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11, …
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.77…
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.92…
```
  
**3. Use `mutate()` Change the variables `HuntCat`, `LandUse`, and `TransectID` to factors.**

``` r
gabon <- gabon %>% 
  mutate(HuntCat = as.factor(HuntCat),
         LandUse = as.factor(LandUse),
         TransectID = as.factor(TransectID))

is.factor(gabon$HuntCat)
```

```
## [1] TRUE
```

``` r
is.factor(gabon$LandUse)
```

```
## [1] TRUE
```

``` r
is.factor(gabon$TransectID)
```

```
## [1] TRUE
```

**4. Use `filter` to make three new dataframes focused only on 1. national parks, 2. logging concessions, and 3. neither. Have a look at the README in the data folder so you understand the variables.**

``` r
national_parks <- filter(gabon, LandUse=="Park")
logging_concessions <- filter(gabon, LandUse=="Logging")
neither <- filter(gabon, LandUse=="Neither")
```

**5. How many transects are recorded for each land use type?**

``` r
table(gabon$LandUse)
```

```
## 
## Logging Neither    Park 
##      13       4       7
```

**6. For which land use type (national parks, logging, or neither) is average all species diversity the greatest?**

``` r
national_parks %>% 
  select(LandUse, Diversity_AllSpecies) %>% 
  filter(LandUse == "Park")
```

```
##   LandUse Diversity_AllSpecies
## 1    Park                2.452
## 2    Park                2.314
## 3    Park                2.288
## 4    Park                2.431
## 5    Park                2.429
## 6    Park                2.566
## 7    Park                2.496
```

``` r
  mean(national_parks$Diversity_AllSpecies)
```

```
## [1] 2.425143
```


``` r
logging_concessions %>% 
  select(LandUse, Diversity_AllSpecies) %>% 
  filter(LandUse == "Logging")
```

```
##    LandUse Diversity_AllSpecies
## 1  Logging                2.006
## 2  Logging                2.229
## 3  Logging                1.966
## 4  Logging                2.270
## 5  Logging                2.161
## 6  Logging                2.134
## 7  Logging                2.363
## 8  Logging                2.188
## 9  Logging                2.269
## 10 Logging                2.369
## 11 Logging                2.319
## 12 Logging                2.368
## 13 Logging                2.381
```

``` r
  mean(logging_concessions$Diversity_AllSpecies)
```

```
## [1] 2.232538
```


``` r
neither %>% 
  select(LandUse, Diversity_AllSpecies) %>% 
  filter(LandUse == "Neither")
```

```
##   LandUse Diversity_AllSpecies
## 1 Neither                2.276
## 2 Neither                2.254
## 3 Neither                2.448
## 4 Neither                2.452
```

``` r
  mean(neither$Diversity_AllSpecies)
```

```
## [1] 2.3575
```
The average all species diversity is the greatest for the national park land use type.

**7. Use `filter` to find the transect that has the highest relative abundance of elephants. What land use type is this? Use `arrange()` to sort your results.** 

``` r
gabon %>% 
  select(TransectID, LandUse, RA_Elephant) %>% 
  filter(RA_Elephant !="0.00") %>% 
  arrange(-RA_Elephant)
```

```
##    TransectID LandUse RA_Elephant
## 1          18 Logging        2.30
## 2           8 Neither        2.20
## 3           2    Park        1.33
## 4           6    Park        1.11
## 5           4    Park        1.00
## 6          13 Logging        0.99
## 7           2    Park        0.86
## 8          21 Neither        0.77
## 9           3 Logging        0.56
## 10         14 Logging        0.52
## 11          7 Logging        0.43
## 12         16 Logging        0.36
## 13         19 Logging        0.36
## 14         15 Neither        0.29
## 15          1    Park        0.00
## 16          5    Park        0.00
## 17          9 Logging        0.00
## 18         17 Neither        0.00
## 19         20 Logging        0.00
## 20         22 Logging        0.00
## 21         24    Park        0.00
## 22         25 Logging        0.00
## 23         26 Logging        0.00
## 24         27 Logging        0.00
```
Transect 18 has the highest relative abundance of elephants with a value of 2.30 and the land use type is Logging.

**8. Use `filter` to find all transects that have greater than 15 tree species or a breast height diameter between 50 and 60cm.  **

``` r
gabon %>% 
  select(TransectID, Veg_Rich, Veg_DBH) %>% 
  filter(Veg_Rich>15 | between(Veg_DBH, 50,60))
```

```
##    TransectID Veg_Rich Veg_DBH
## 1           1    16.67   49.57
## 2           2    15.75   34.59
## 3           2    16.88   42.82
## 4           4    17.13   41.52
## 5           5    16.50   44.07
## 6           6    14.75   51.22
## 7           9    16.00   69.30
## 8          14    15.75   52.12
## 9          15    10.88   54.77
## 10         17    14.25   57.71
## 11         21    16.25   50.36
## 12         22    17.13   39.31
## 13         24    16.75   44.21
## 14         26    18.75   35.58
```

**9.Which transects and land use types have more than 10 tree species and 10 mammal species? Use `arrange()` to sort by the number of tree species.**

``` r
gabon %>% 
  select(TransectID, LandUse, Veg_Rich, Rich_MammalSpecies) %>% 
  filter(Veg_Rich>10 & Rich_MammalSpecies==10)
```

```
##   TransectID LandUse Veg_Rich Rich_MammalSpecies
## 1          2    Park    15.75                 10
## 2          9 Logging    16.00                 10
## 3         13 Logging    12.38                 10
## 4         21 Neither    16.25                 10
## 5         25 Logging    15.00                 10
## 6         26 Logging    18.75                 10
```

**10. Explore the data! Develop one question on your own that includes at least two lines of code. **
Question: Do national parks have a higher average Bird Diversity or a higher average mammal diversity?

``` r
national_parks %>% 
  select(TransectID, LandUse, Diversity_BirdSpecies) %>% 
  filter(LandUse == "Park") 
```

```
##   TransectID LandUse Diversity_BirdSpecies
## 1          1    Park                 1.756
## 2          2    Park                 1.620
## 3          2    Park                 1.649
## 4          4    Park                 1.660
## 5          5    Park                 1.775
## 6          6    Park                 1.920
## 7         24    Park                 2.008
```

``` r
  mean(national_parks$Diversity_BirdSpecies)
```

```
## [1] 1.769714
```

``` r
national_parks %>% 
  select(TransectID, LandUse, Diversity_MammalSpecies) %>% 
  filter(LandUse == "Park") 
```

```
##   TransectID LandUse Diversity_MammalSpecies
## 1          1    Park                   1.764
## 2          2    Park                   1.624
## 3          2    Park                   1.558
## 4          4    Park                   1.829
## 5          5    Park                   1.725
## 6          6    Park                   1.928
## 7         24    Park                   1.810
```

``` r
  mean(national_parks$Diversity_MammalSpecies)
```

```
## [1] 1.748286
```
There is a higher average bird diversity in national parks.

## Knit and Upload
Please knit your work as a .pdf or .html file and upload to Canvas. Homework is due before the start of the next lab. No late work is accepted. Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  
