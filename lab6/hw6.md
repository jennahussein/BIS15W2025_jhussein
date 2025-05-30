---
title: "Homework 6"
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

## Load the superhero data
Let's have a little fun with this one! We are going to explore data on superheroes. These are data taken from comic books and assembled by devoted fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Load the `heroes_information.csv` and `super_hero_powers.csv` data. Make sure the columns are cleanly named.

``` r
superhero_info <- read_csv("data/heroes_information.csv", na = c("", "-99", "-")) %>% clean_names()
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
superhero_powers <- read_csv("data/super_hero_powers.csv", na = c("", "-99", "-")) %>% clean_names()
```

```
## Rows: 667 Columns: 168
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. For the superhero_info data, how many bad, good, and neutral superheros are there? Try using count() and/ or tabyl().

``` r
superhero_info %>% 
  count(alignment)
```

```
## # A tibble: 4 × 2
##   alignment     n
##   <chr>     <int>
## 1 bad         207
## 2 good        496
## 3 neutral      24
## 4 <NA>          7
```


``` r
superhero_info %>% 
  tabyl(alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some bad superheros! Who are they? List their names below.  

``` r
superhero_info %>% 
  select(name, alignment) %>% 
  filter(alignment=="bad")
```

```
## # A tibble: 207 × 2
##    name          alignment
##    <chr>         <chr>    
##  1 Abomination   bad      
##  2 Abraxas       bad      
##  3 Absorbing Man bad      
##  4 Air-Walker    bad      
##  5 Ajax          bad      
##  6 Alex Mercer   bad      
##  7 Alien         bad      
##  8 Amazo         bad      
##  9 Ammo          bad      
## 10 Angela        bad      
## # ℹ 197 more rows
```

3. How many distinct "races" are represented in `superhero_info`?

``` r
superhero_info %>% 
  summarize(n_races=n_distinct(race, na.rm=T))
```

```
## # A tibble: 1 × 1
##   n_races
##     <int>
## 1      61
```

## Good and Bad
4. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

``` r
good_guys <- superhero_info %>% 
  filter(alignment=="good")
```


``` r
bad_guys <- superhero_info %>% 
  filter(alignment=="bad")
```

5. Who are the good Vampires?

``` r
good_guys %>% 
  select(name, race, alignment) %>% 
  filter(race=="Vampire")
```

```
## # A tibble: 2 × 3
##   name  race    alignment
##   <chr> <chr>   <chr>    
## 1 Angel Vampire good     
## 2 Blade Vampire good
```

6. Who has the height advantage- bad guys or good guys? Convert their height to meters and sort from tallest to shortest.  

``` r
good_guys %>% 
  mutate(height_meters = height/100) %>% 
  select(name, height, height_meters) %>% 
  arrange(-height_meters)
```

```
## # A tibble: 496 × 3
##    name          height height_meters
##    <chr>          <dbl>         <dbl>
##  1 Fin Fang Foom   975           9.75
##  2 Groot           701           7.01
##  3 Wolfsbane       366           3.66
##  4 Sasquatch       305           3.05
##  5 Ymir            305.          3.05
##  6 Rey             297           2.97
##  7 Hellboy         259           2.59
##  8 Hulk            244           2.44
##  9 Kilowog         234           2.34
## 10 Cloak           226           2.26
## # ℹ 486 more rows
```


``` r
bad_guys %>% 
  mutate(height_meters = height/100) %>% 
  select(name, height, height_meters) %>% 
  arrange(-height_meters)
```

```
## # A tibble: 207 × 3
##    name           height height_meters
##    <chr>           <dbl>         <dbl>
##  1 MODOK             366          3.66
##  2 Onslaught         305          3.05
##  3 Sauron            279          2.79
##  4 Solomon Grundy    279          2.79
##  5 Darkseid          267          2.67
##  6 Amazo             257          2.57
##  7 Alien             244          2.44
##  8 Doomsday          244          2.44
##  9 Killer Croc       244          2.44
## 10 Venom III         229          2.29
## # ℹ 197 more rows
```
The good guys have a height advantage over the bad guys. The tallest good superhero is 9.75 meters and the tallest bad superhero is 3.66 meters.

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

7. How many superheros have a combination of agility, stealth, super_strength, and stamina?

``` r
superhero_powers %>% 
  select(hero_names, agility, stealth, super_strength, stamina) %>% 
  filter(agility=="TRUE", stealth=="TRUE", super_strength=="TRUE", stamina=="TRUE") %>% 
  summarize(total=n())
```

```
## # A tibble: 1 × 1
##   total
##   <int>
## 1    40
```



8. Who is the most powerful superhero? Have a look at the code chunk below. Use the internet to annotate each line of code so you know how it works. It's OK to use AI to help you with this task.

``` r
superhero_powers %>%
  #calls the superhero_powers data frame
  mutate(across(-1, ~ ifelse(. == TRUE, 1, 0))) %>% 
  #mutate all variables and observations within each variable except the first variable. The period tells R to evaluate all cells. If a cell says True, change its value to 1. If a cell says False, change its value to 0.
  mutate(total_powers = rowSums(across(-1))) %>% 
  #make a new column called total_powers and have it's value be a sum of all the rows except for the first row.
  select(hero_names, total_powers) %>% 
  #pull out only the hero_names and total_powers columns/variables from the data frame.
  arrange(-total_powers)
```

```
## # A tibble: 667 × 2
##    hero_names        total_powers
##    <chr>                    <dbl>
##  1 Spectre                     49
##  2 Amazo                       44
##  3 Living Tribunal             35
##  4 Martian Manhunter           35
##  5 Man of Miracles             34
##  6 Captain Marvel              33
##  7 T-X                         33
##  8 Galactus                    32
##  9 T-1000                      32
## 10 Mister Mxyzptlk             31
## # ℹ 657 more rows
```

``` r
  #sort the total_powers data in reverse order
```

## Your Favorite
9. Pick your favorite superhero and let's see their powers!  

``` r
superhero_powers %>% 
  filter(hero_names=="Batman")
```

```
## # A tibble: 1 × 168
##   hero_names agility accelerated_healing lantern_power_ring
##   <chr>      <lgl>   <lgl>               <lgl>             
## 1 Batman     TRUE    FALSE               FALSE             
## # ℹ 164 more variables: dimensional_awareness <lgl>, cold_resistance <lgl>,
## #   durability <lgl>, stealth <lgl>, energy_absorption <lgl>, flight <lgl>,
## #   danger_sense <lgl>, underwater_breathing <lgl>, marksmanship <lgl>,
## #   weapons_master <lgl>, power_augmentation <lgl>, animal_attributes <lgl>,
## #   longevity <lgl>, intelligence <lgl>, super_strength <lgl>,
## #   cryokinesis <lgl>, telepathy <lgl>, energy_armor <lgl>,
## #   energy_blasts <lgl>, duplication <lgl>, size_changing <lgl>, …
```

10. Can you find your hero in the superhero_info data? Show their info!  

``` r
superhero_info %>% 
  filter(name=="Batman")
```

```
## # A tibble: 2 × 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Batman Male   blue      Human black         188 DC Comics <NA>       good     
## 2 Batman Male   blue      Human Black         178 DC Comics <NA>       good     
## # ℹ 1 more variable: weight <dbl>
```

## Knit and Upload
Please knit your work as a .pdf or .html file and upload to Canvas. Homework is due before the start of the next lab. No late work is accepted. Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  
