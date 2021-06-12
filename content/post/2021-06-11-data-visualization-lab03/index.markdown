---
title: Data Visualization Lab03
author: Christopher P. Loo
date: '2021-06-11'
slug: data-visualization-lab03
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-06-11T23:28:29-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


# Libraries


```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
## ✓ tibble  3.1.1     ✓ dplyr   1.0.6
## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(readr)
library(wordbankr)
```

```
## Warning in .recacheSubclasses(def@className, def, env): undefined subclass
## "numericVector" of class "Mnumeric"; definition not updated
```

```r
library(here)
```

```
## here() starts at /Users/looc/Box/2021_Spring_classes/Data_Viz/blogdown/Chris_Loo_website
```

```r
library(RColorBrewer)
library(wesanderson)
library(ggthemes)
library(ggsci)
library(beyonce)
```

```
## Registered S3 method overwritten by 'beyonce':
##   method        from       
##   print.palette wesanderson
```

```r
library(viridis)
```

```
## Loading required package: viridisLite
```

```r
library(forcats)
library(colorspace)
library(colorblindr)
library(ggrepel)
library(ggplot2)
```

## And a few from Github...


```r
# Note that I have already taken care of installing these in this RStudio Cloud project, but if you want to use these libraries yourself, here's how you'd install them:
library(devtools)
devtools::install_github("wilkelab/cowplot")
devtools::install_github("clauswilke/colorblindr")
devtools::install_github("dill/beyonce")
```


# Reading the data

We'll be working with `data/animal_sounds_summary.csv`...

# Know your data! (Challenge #1)

```r
data <- read.csv('data/animal_sounds_summary.csv')
```


```r
#install.packages("skimr")
library(skimr)
skim(data)
```


Table: Table 1: Data summary

|                         |     |
|:------------------------|:----|
|Name                     |data |
|Number of rows           |33   |
|Number of columns        |7    |
|_______________________  |     |
|Column type frequency:   |     |
|character                |1    |
|numeric                  |6    |
|________________________ |     |
|Group variables          |None |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|sound         |         0|             1|   4|  14|     0|        3|          0|


**Variable type: numeric**

|skim_variable   | n_missing| complete_rate|   mean|     sd|    p0|   p25|    p50|    p75|   p100|hist  |
|:---------------|---------:|-------------:|------:|------:|-----:|-----:|------:|------:|------:|:-----|
|age             |         0|             1|  13.00|   3.21|  8.00| 10.00|  13.00|  16.00|  18.00|▇▅▅▅▅ |
|kids_produce    |         0|             1|  53.61| 101.49|  0.00|  3.00|  12.00|  54.00| 439.00|▇▁▁▁▁ |
|kids_understand |         0|             1| 113.52| 173.43|  2.00| 22.00|  43.00|  78.00| 625.00|▇▁▁▁▁ |
|kids_respond    |         0|             1| 218.97| 253.32| 35.00| 89.00| 118.00| 141.00| 769.00|▇▁▁▁▂ |
|prop_produce    |         0|             1|   0.21|   0.23|  0.00|  0.02|   0.13|   0.31|   0.71|▇▂▁▁▂ |
|prop_understand |         0|             1|   0.44|   0.27|  0.02|  0.24|   0.42|   0.66|   0.89|▇▇▆▆▆ |

## How many variables?
Six variables

### Which are continuous 

```r
glimpse(data)
```

```
## Rows: 33
## Columns: 7
## $ age             <int> 8, 8, 8, 9, 9, 9, 10, 10, 10, 11, 11, 11, 12, 12, 12, …
## $ sound           <chr> "cockadoodledoo", "meow", "woof woof", "cockadoodledoo…
## $ kids_produce    <int> 1, 0, 3, 0, 2, 2, 0, 5, 4, 0, 5, 12, 0, 12, 28, 9, 125…
## $ kids_understand <int> 3, 10, 12, 2, 21, 22, 9, 41, 40, 4, 36, 32, 16, 59, 59…
## $ kids_respond    <int> 35, 35, 35, 91, 93, 93, 139, 145, 143, 94, 94, 94, 141…
## $ prop_produce    <dbl> 0.02857143, 0.00000000, 0.08571429, 0.00000000, 0.0215…
## $ prop_understand <dbl> 0.08571429, 0.28571429, 0.34285714, 0.02197802, 0.2258…
```
I think only age is a continuous variable in this data set.
However, since proportions can be along continuous values from 0-100 then prop_produce and prop_understand
is also continuous variables. 

### Which are categorical and ordinal?
sound is a categorical variable.
kids_produce and kids_respond are more ordinal variables as they can be divided into range groups.

## How many _total_ kids?

```r
nrow(data)
```

```
## [1] 33
```


## How many different ages?

```r
nrow(distinct(data))
```

```
## [1] 33
```

### How many kids per age?

```r
data %>% count(age)
```

```
##    age n
## 1    8 3
## 2    9 3
## 3   10 3
## 4   11 3
## 5   12 3
## 6   13 3
## 7   14 3
## 8   15 3
## 9   16 3
## 10  17 3
## 11  18 3
```

## How many types of animal sounds, and what are they?

```r
data %>% count(sound)
```

```
##            sound  n
## 1 cockadoodledoo 11
## 2           meow 11
## 3      woof woof 11
```

# Initial EDA Plots

## How many kids produce each kind of sound?

```r
ggplot(data, aes(x=sound, y=kids_produce)) + geom_col()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="672" />

```r
# or a table, which is nice since I usually don't make tables.
data %>% 
  group_by(sound) %>%
  summarize(total_produce = sum(kids_produce)) %>%
  knitr::kable()
```



|sound          | total_produce|
|:--------------|-------------:|
|cockadoodledoo |           148|
|meow           |           681|
|woof woof      |           940|

## Adding age


```r
ggplot(data, aes(x = age, y = prop_produce)) +
  geom_col() +
  labs(x = "Age", y = "Proportion of Children Producing")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="672" />

### Bar chart

```r
ggplot(data, aes(x = age, y = prop_produce)) +
  geom_col() +
  labs(x = "Age", y = "Proportion of Children Producing") +
  facet_wrap(~sound)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-1.png" width="672" />
### Scatter plot

```r
ggplot(data, aes(x = age, y = prop_produce)) +
  geom_point() +
  labs(x = "Age (in months)", y = "Proportion of Children Producing") +
  facet_wrap(~sound)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-14-1.png" width="672" />

# Discrete colors


## Initial (uncolored) plot

_Remember: Make sure to adjust the labels!!_


```r
ggplot(data, aes(x = age, y = prop_produce)) +
  geom_point() +
  labs(x = "Age (months)", y = "Proportion of Children Producing") 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />


## Default discrete palette (Challenge #2)

```r
ggplot(data, aes(x = age, y = prop_produce)) +
  geom_point(aes(color = sound)) +
  labs(x = "Age (months)", y = "Proportion of Children Producing") 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="672" />

## Adding lines (Challenge #3)

```r
ggplot(data, aes(x = age, y = prop_produce)) +
  geom_point(aes(color = sound), size = 2) +
  geom_line() +
  labs(x = "Age (months)", y = "Proportion of Children Producing")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-17-1.png" width="672" />
**Interestingly, if I add my geom_line() after geom_point() it effects the layers of plot. The black line is layered over the colored data points.**



```r
ggplot(data, aes(x = age, y = prop_produce)) +
  geom_line(aes(group = sound)) +
  geom_point(aes(color = sound), size = 2) +
   labs(x = "Age (months)", y = "Proportion of Children Producing")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="672" />

## Challenge #4


### Coloring both lines and points

```r
ggplot(data, aes(x = age, y = prop_produce, color = sound)) + 
  geom_line() +
  geom_point(size = 2) +
  labs(x = "Age (months)", y = "Proportion of Children Producing") 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-1.png" width="672" />

### Using `geom_smooth()`

```r
ggplot(data, aes(x = age, y = prop_produce, color = sound)) + 
  geom_smooth(se = FALSE, lwd = .5) +
  #lwd is line weight, se = FALSE removes the standard error around the line
  geom_point(size = 2) +
  labs(x = "Age (months)", y = "Proportion of Children Producing") 
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-20-1.png" width="672" />
## Controlling factor order

```r
#install.packages("forcats")   #forcats package: for cat(egorical)
library(forcats)
```


```r
sounds <- data %>% 
  mutate(data = as.factor(sound))

sound_traj <- ggplot(sounds, aes(x = age, y = prop_produce,
                                color = fct_reorder2(sound, age, prop_produce))) +
  geom_smooth(se = FALSE, lwd = .5) +
  geom_point(size = 2) +
  labs(x = "Age (months)", y = "Proportion of Children Producing", color = "sound")
sound_traj
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-22-1.png" width="672" />

## Modifying default colors

Experiment with each property in `scale_color_hue()` to get a sense of what it does.

```r
sound_traj + 
  scale_color_hue()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-23-1.png" width="672" />

```r
sound_traj +
  scale_color_hue(h = c(0, 90), l = 65, c = 100)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-24-1.png" width="672" />

```r
sound_traj +
  scale_color_hue(l = 45)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-25-1.png" width="672" />


```r
sound_traj +
  scale_color_hue(l = 75, c = 50)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-26-1.png" width="672" />

## Setting discrete colors

Experiment with `scale_color_manual()` and some of the various [named colors](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf) that come built-in to R!


```r
sound_traj +
  scale_color_manual(values = c("cornflowerblue", "seagreen", "coral"))
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-27-1.png" width="672" />


### Challenge #5

Why doesn't the code block change the colors?

sound_traj <- ggplot(sounds, aes(x = age, y = prop_produce,
                                color = fct_reorder2(sound, age, prop_produce))) +
  geom_smooth(se = FALSE, lwd = .5) +
  geom_point(size = 2) +
  labs(x = "Age (months)", y = "Proportion of Children Producing", color = "sound")
sound_traj


```r
ggplot(sounds, aes(x = age, 
                         y = prop_produce, 
                         color = fct_reorder2(sound, age, prop_produce))) + 
  geom_smooth(se = FALSE, lwd = .5) +
  geom_point(size = 2) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing", 
       color = "sound") +
  scale_fill_manual(values = c("cornflowerblue", 
                               "seagreen", "coral"))
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-28-1.png" width="672" />

```r
ggplot(sounds, aes(x = age, 
                         y = prop_produce, 
                         fill = fct_reorder2(sound, age, prop_produce))) + 
  geom_smooth(se = FALSE, lwd = .5) +
  geom_point(size = 2) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing", 
       fill = "sound") +
  scale_fill_manual(values = c("cornflowerblue", 
                               "seagreen", "coral"))
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-29-1.png" width="672" />
**Answers:**
In the first, we used scale_fill_manual, but the in the global aesthetics, we **mapped the color, not fill**, aesthetic onto the sound variable.
    In the second, we did define the fill aesthetic and used scale_fill_manual, so that is good. But **geom_line only understands the color aesthetic, not fill**. And for geom_point, the default shape for is 19, which does not understand the fill aesthetic.
    
### Challenge #6


```r
ggplot(sounds, aes(x = age, y = prop_produce, fill = fct_reorder2(sound, age, prop_produce))) +
  geom_smooth(aes(color = fct_reorder2(sound, age, prop_produce)), se = FALSE, lwd = .5,
              show.legend = FALSE) +
  geom_point(size = 2, shape = 21) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing", 
       fill = "sound")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-30-1.png" width="672" />

```r
ggplot(sounds, aes(x = age, y = prop_produce, fill = fct_reorder2(sound, age, prop_produce))) +
  geom_smooth(aes(color = fct_reorder2(sound, age, prop_produce)), se = FALSE, lwd = .5,
              show.legend = FALSE) +
  geom_point(size = 2, shape = 21) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing", 
       fill = "sound") +
  scale_fill_manual(values = c("cornflowerblue", "seagreen", "coral")) +
  scale_color_manual(values = c("cornflowerblue", "seagreen", "coral"))
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-31-1.png" width="672" />

```r
my_colors <- c("cadetblue", "steelblue", "salmon") 

sound_traj +
  scale_color_manual(values = my_colors)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-32-1.png" width="672" />


### Challenge #7


```r
sb_colorblind <- c("#000000", "#009E73", "#911eb4",
                        "#CC79A7", "#F0E442", "#56B4E9")
sound_traj +
  scale_colour_manual(values = sb_colorblind)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-33-1.png" width="672" />

## Built-in descrete color palettes

### Using `scale_color_brewer()`

```r
#install.packages("RColorBrewer")
library(RColorBrewer)
```


```r
brewer.pal(5, "Dark2")
```

```
## [1] "#1B9E77" "#D95F02" "#7570B3" "#E7298A" "#66A61E"
```

```r
sound_traj +
  scale_color_brewer(palette = "Dark2")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-36-1.png" width="672" />

### Using `wesanderson`

```r
#install.packages("wesanderson")
library(wesanderson)
```


```r
names(wes_palettes)
```

```
##  [1] "BottleRocket1"  "BottleRocket2"  "Rushmore1"      "Rushmore"      
##  [5] "Royal1"         "Royal2"         "Zissou1"        "Darjeeling1"   
##  [9] "Darjeeling2"    "Chevalier1"     "FantasticFox1"  "Moonrise1"     
## [13] "Moonrise2"      "Moonrise3"      "Cavalcanti1"    "GrandBudapest1"
## [17] "GrandBudapest2" "IsleofDogs1"    "IsleofDogs2"
```

```r
wes_palette("Zissou1")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-39-1.png" width="672" />

```r
sound_traj +
  scale_color_manual(values = wes_palette("Darjeeling1"))
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-40-1.png" width="672" />

```r
sound_traj +
  scale_color_manual(values = wes_palette("BottleRocket2"))
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-41-1.png" width="672" />


#### Challenge #8

```r
sound_traj +
  scale_color_manual(values = wes_palette("Rushmore")[3:5])
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-42-1.png" width="672" />

```r
sound_traj +
  scale_color_manual(values = wes_palette("Zissou1")[c(2, 3, 5)])
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-43-1.png" width="672" />

### Using `ggthemes`

```r
#install.packages("ggthemes")
library(ggthemes)
```


```r
sound_traj +
  scale_color_fivethirtyeight()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-45-1.png" width="672" />

```r
sound_traj +
  scale_color_economist()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-46-1.png" width="672" />

```r
library(ggsci)

sound_traj + scale_color_nejm()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-47-1.png" width="672" />

### Using `beyonce`

```r
#install.packages("devtools")
#devtools::install_github("dill/beyonce")
library(beyonce)
```


```r
beyonce_palette(18)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-49-1.png" width="672" />

```r
sound_traj +
  scale_color_manual(values = beyonce_palette(18)[3:5])
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-50-1.png" width="672" />

### Using `viridis`

```r
#install.packages("viridis")
library(viridis)
```

The default argument for discrete is FALSE, so to use the discrete palettes you need to set discrete = TRUE. 


```r
sound_traj +
  scale_color_viridis(discrete = TRUE) +
  theme_minimal()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-52-1.png" width="672" />

```r
sound_traj +
  scale_color_viridis(discrete = TRUE, option = "plasma") +
  theme_minimal()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-53-1.png" width="672" />

#### Challenge #9

## Greyscale

Experimenting with `scale_color_grey()`/`scale_fill_grey()`

```r
ggplot(sounds, aes(x = age, y = prop_produce, fill = fct_reorder2(sound, age, prop_produce))) +
  geom_smooth(aes(color = fct_reorder2(sound, age, prop_produce)), 
              se = FALSE, lwd = .5, show.legend = FALSE) +
  geom_point(size = 2, shape = 21, colour = "midnightblue") +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing", 
       fill = "sound") +
  scale_fill_viridis(discrete = TRUE) +
  scale_color_viridis(discrete = TRUE) +
  theme_minimal()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-54-1.png" width="672" />


```r
sound_traj +
  scale_color_grey() +
  theme_minimal()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-55-1.png" width="672" />

```r
sound_traj +
  scale_color_grey(start = 0.2, end = .8)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-56-1.png" width="672" />


```r
ggplot(sounds, aes(x = age, 
                   y = prop_produce, 
                   fill = fct_reorder2(sound, age, prop_produce))) + 
  geom_smooth(aes(color = fct_reorder2(sound, age, prop_produce)),
              se = FALSE, lwd = .5, show.legend = FALSE) +
  geom_point(size = 2, shape = 21) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing", 
       fill = "sound") +
  scale_fill_grey(start = 0.3, end = 1) +
  scale_color_grey(start = 0.3, end = 1) 
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-57-1.png" width="672" />


```r
ggplot(sounds, aes(x = age, 
                   y = prop_produce, 
                   fill = fct_reorder2(sound, age, prop_produce))) + 
  geom_smooth(aes(lty = fct_reorder2(sound, age, prop_produce)), color = "black",
              se = FALSE, lwd = .5, show.legend = FALSE) +
  geom_point(size = 2, shape = 21) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing", 
       fill = "sound") +
  scale_fill_grey(start = 0.3, end = 1) 
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-58-1.png" width="672" />

```r
ggplot(sounds, aes(x = age, 
                   y = prop_produce, 
                   fill = fct_reorder2(sound, age, prop_produce))) + 
  geom_smooth(aes(color = fct_reorder2(sound, age, prop_produce),
                  lty = fct_reorder2(sound, age, prop_produce)),
              se = FALSE, lwd = .5, show.legend = FALSE) +
  geom_point(size = 2, shape = 21) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing", 
       fill = "sound") +
  scale_fill_grey(start = 0.3, end = .8) +
  scale_color_grey(start = 0.3, end = .8) 
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-59-1.png" width="672" />

## Using `colorblindr`

```r
#devtools::install_github("wilkelab/cowplot)
#install:packages("colorspace", repos = "http://R-Forge.R-project.org")
#devtools::install_github("clauswilke/colorblindr")
```


```r
my_sound_traj <- sound_traj +
  scale_color_manual(values = beyonce_palette(18)[c(1, 4, 5)])
```


```r
library(colorblindr)
cvd_grid(my_sound_traj)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-62-1.png" width="672" />


```r
cb_sound_traj <- sound_traj +
  scale_color_OkabeIto()
cb_sound_traj
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-63-1.png" width="672" />

```r
cvd_grid(cb_sound_traj)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-64-1.png" width="672" />

```r
cbbPalette <- c("#000000", "#E69F00", "#56B4E9", "#009E73", "#F0E442", "#0072B2", "#D55E00", "#CC79A7")

# To use for line and point colors, add
sound_traj +
  scale_colour_manual(values = cbbPalette[c(3, 7, 8)])
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-65-1.png" width="672" />

## Repel labels

```r
library(ggrepel)

sounds <- sounds %>%
  mutate(label = case_when(
    age == max(age) ~ sound))

ggplot(sounds, aes(x = age, 
                   y = prop_produce, 
                   color = fct_reorder2(sound, age, prop_produce))) +
  geom_smooth(se = FALSE, lwd = .5) +
  geom_point(size = 2) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing") +
  geom_text_repel(aes(label = label),
                  nudge_x = 1,
                  direction = "y",
                  na.rm = TRUE) +
  guides(color = FALSE)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-66-1.png" width="672" />

# Continuous COlors

Experiment a bit with `scale_color_gradient()`/`scale_fill_gradient()`!

## Built-in continuous palettes

Experiment a bit with `RColorBrewer` and `viridis`

```r
sound_by_age <- ggplot(sounds, aes(x = age, 
                                   y = prop_produce, 
                                   color = age)) +
  geom_line(aes(group = sound), lwd = .5) +
  geom_point(size = 2) +
  labs(x = "Age (months)", 
       y = "Proportion of Children Producing")
sound_by_age
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-67-1.png" width="672" />

```r
sound_by_age +
  scale_color_gradient(trans = "reverse")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-68-1.png" width="672" />

```r
sound_by_age +
  scale_color_gradient(low = "white", high = "red")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-69-1.png" width="672" />

```r
sound_by_age +
  scale_color_gradient(low = "grey90", high = "black")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-70-1.png" width="672" />

```r
med_age <- sounds %>% 
  summarize(mos = median(age)) %>% 
  pull()
sound_by_age +
  scale_color_gradient2(midpoint = med_age,
                      low="blue", mid="white", high="red" )
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-71-1.png" width="672" />


# Challenge #10

```r
#install.packages("gapminder")
library(gapminder)
```


```r
#load data
gap <- gapminder
glimpse(gap)
```

```
## Rows: 1,704
## Columns: 6
## $ country   <fct> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", …
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, 1997, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854, 40.8…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 14880372, 12…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.1134, …
```

```r
skim(gap)
```


Table: Table 2: Data summary

|                         |     |
|:------------------------|:----|
|Name                     |gap  |
|Number of rows           |1704 |
|Number of columns        |6    |
|_______________________  |     |
|Column type frequency:   |     |
|factor                   |2    |
|numeric                  |4    |
|________________________ |     |
|Group variables          |None |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts                             |
|:-------------|---------:|-------------:|:-------|--------:|:--------------------------------------|
|country       |         0|             1|FALSE   |      142|Afg: 12, Alb: 12, Alg: 12, Ang: 12     |
|continent     |         0|             1|FALSE   |        5|Afr: 624, Asi: 396, Eur: 360, Ame: 300 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|        mean|           sd|       p0|        p25|        p50|         p75|         p100|hist  |
|:-------------|---------:|-------------:|-----------:|------------:|--------:|----------:|----------:|-----------:|------------:|:-----|
|year          |         0|             1|     1979.50|        17.27|  1952.00|    1965.75|    1979.50|     1993.25|       2007.0|▇▅▅▅▇ |
|lifeExp       |         0|             1|       59.47|        12.92|    23.60|      48.20|      60.71|       70.85|         82.6|▁▆▇▇▇ |
|pop           |         0|             1| 29601212.32| 106157896.74| 60011.00| 2793664.00| 7023595.50| 19585221.75| 1318683096.0|▇▁▁▁▁ |
|gdpPercap     |         0|             1|     7215.33|      9857.45|   241.17|    1202.06|    3531.85|     9325.46|     113523.1|▇▁▁▁▁ |


```r
sum(is.na(gap))
```

```
## [1] 0
```


```r
summary(gap)
```

```
##         country        continent        year         lifeExp     
##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
##  Australia  :  12                  Max.   :2007   Max.   :82.60  
##  (Other)    :1632                                                
##       pop              gdpPercap       
##  Min.   :6.001e+04   Min.   :   241.2  
##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
##  Median :7.024e+06   Median :  3531.8  
##  Mean   :2.960e+07   Mean   :  7215.3  
##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
##  Max.   :1.319e+09   Max.   :113523.1  
## 
```

```r
unique(gap %>% select(year, country, continent))
```

```
## # A tibble: 1,704 x 3
##     year country     continent
##    <int> <fct>       <fct>    
##  1  1952 Afghanistan Asia     
##  2  1957 Afghanistan Asia     
##  3  1962 Afghanistan Asia     
##  4  1967 Afghanistan Asia     
##  5  1972 Afghanistan Asia     
##  6  1977 Afghanistan Asia     
##  7  1982 Afghanistan Asia     
##  8  1987 Afghanistan Asia     
##  9  1992 Afghanistan Asia     
## 10  1997 Afghanistan Asia     
## # … with 1,694 more rows
```

## Version with _good_ color

Also, good with default colors


```r
brewer.pal(5, "Dark2") # list 5 hex colors
```

```
## [1] "#1B9E77" "#D95F02" "#7570B3" "#E7298A" "#66A61E"
```

```r
my_plot <- ggplot(gap, aes(x = log(gdpPercap), y = lifeExp, color = (continent))) + 
  geom_point(alpha = .2, size = 1) +
  labs(x = "GDP Per Capita (log)", y = "Life Expectancy") +
  facet_wrap(~continent)
my_plot + scale_color_brewer(palette = "Dark2")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-78-1.png" width="672" />


## Version with greyscale


```r
my_plot + scale_color_grey(start = 0.2, end = .8) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-79-1.png" width="672" />

## Version with _dreadful_ color

```r
my_plot +
  scale_color_manual(values = wes_palette("Zissou1"))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-80-1.png" width="672" />
